<script lang="ts">
  import type { SystemConfigDto } from '@immich/sdk';
  import { isEqual } from 'lodash-es';
  import { createEventDispatcher } from 'svelte';
  import { fade } from 'svelte/transition';
  import type { SettingsEventType } from '../admin-settings';
  import SettingButtonsRow from '$lib/components/shared-components/settings/setting-buttons-row.svelte';
  import SettingSwitch from '$lib/components/shared-components/settings/setting-switch.svelte';

  export let savedConfig: SystemConfigDto;
  export let defaultConfig: SystemConfigDto;
  export let config: SystemConfigDto; // this is the config that is being edited
  export let disabled = false;

  const dispatch = createEventDispatcher<SettingsEventType>();
</script>

<div>
  <div in:fade={{ duration: 500 }}>
    <form autocomplete="off" on:submit|preventDefault>
      <div class="ml-4 mt-4">
        <SettingSwitch
          id="enable-new-version-check"
          title="ENABLED"
          subtitle="Enable periodic requests to GitHub to check for new releases"
          bind:checked={config.newVersionCheck.enabled}
          {disabled}
        />
        <SettingButtonsRow
          on:reset={({ detail }) => dispatch('reset', { ...detail, configKeys: ['newVersionCheck'] })}
          on:save={() => dispatch('save', { newVersionCheck: config.newVersionCheck })}
          showResetToDefault={!isEqual(savedConfig, defaultConfig)}
          {disabled}
        />
      </div>
    </form>
  </div>
</div>
