<template>
  <div class="menu-actions-row">
    <Tooltip :content="keymapShortcutString"
             placement="bottom"
             :transfer="true"
             class="tooltip-height">
        <span @click="showKeymap">
            <img src="./../../assets/icon/keymap.svg" alt="Keymap" class="action-icon"/>
        </span>
    </Tooltip>

    <Tooltip :content="$t('menu.synchronization')"
             placement="bottom"
             v-if="showIsSyncing"
             :transfer="true"
             class="tooltip-height">
      <CloudSyncIcon class="action-icon"/>
    </Tooltip>

    <Tooltip :content="$t('menu.cloud_not_connected')"
             placement="bottom"
             v-if="showNotConnectedIcon"
             :transfer="true"
             class="tooltip-height">
      <img src="./../../assets/icon/cloud_unset.svg"
           @click="showCloudModal"
           alt="Cloud - not connected"
           class="action-icon"/>
    </Tooltip>

    <Tooltip :content="$t('menu.cloud_connected')"
             placement="bottom"
             v-if="showConnectedIcon"
             :transfer="true"
             class="tooltip-height">
      <img src="./../../assets/icon/cloud_connected.svg"
           @click="showCloudModal"
           class="action-icon"/>
    </Tooltip>

    <Tooltip :content="$t('menu.cloud_connection_error')"
             placement="bottom"
             v-if="showConnectionErrorIcon"
             :transfer="true"
             class="tooltip-height">
      <img src="./../../assets/icon/cloud_error.svg"
           @click="showCloudModal"
           class="action-icon"/>
    </Tooltip>

    <Tooltip :content="$t('menu.app_settings')"
             placement="bottom"
             :transfer="true"
             class="tooltip-height">
      <img src="./../../assets/icon/settings.svg"
           class="action-icon"
           @click="showSettingsModal"/>
    </Tooltip>

    <Tooltip content="lock"
             placement="bottom"
             :transfer="true"
             class="tooltip-height">
      <img src="./../../assets/icon/lock.svg"
           class="action-icon lock"
           @click="lock"/>
    </Tooltip>
  </div>
</template>

<script>
  import keyShortcutMixin from "./../../keyShortcutStringMixin";
  import CloudSyncIcon from "./../../assets/icon/CloudSyncIcon";

  export default {
    name: "MenuActionsRow",
    mixins: [keyShortcutMixin],
    components: {CloudSyncIcon},
    computed: {
      showNotConnectedIcon() {
        return !this.hasToken && !this.connectionError && !this.showIsSyncing;
      },
      showConnectedIcon() {
        return this.hasToken && !this.connectionError && !this.showIsSyncing;
      },
      showConnectionErrorIcon() {
        return this.connectionError && !this.showIsSyncing;
      },
      showIsSyncing() {
        return this.isSyncing;
      },
      isSyncing() {
        return this.$store.state.cloud.syncInProgress;
      },
      connectionError() {
        return this.$store.state.cloud.connectionError || this.$store.state.cloud.syncError;
      },
      hasToken() {
        return !!this.$store.state.cloud.token;
      },
      keymapShortcutString() {
        return `${this.$t('menu.keymap')} - ${this.shortcutString("showKeymap")}`;
      }
    },
    methods: {
      lock() {
        this.$store.commit("SET_AUTHENTICATED", false);
      },
      showKeymap() {
        this.$store.dispatch("showKeymapModal");
      },
      showSettingsModal() {
        this.$store.dispatch("showSettingsModal");
      },
      showCloudModal() {
        this.$store.dispatch("showCloudModal");
      }
    }
  };
</script>

<style scoped>
  .menu-actions-row {
    padding: 8px 32px;
    display: flex;
    position: absolute;
    bottom: 0;
    left: 0;
    justify-content: space-around;
    align-items: center;
    background-color: #404c5a;
    width: 100%;
    user-select: none;
  }

  .tooltip-height {
    height: 24px;
  }

  .action-icon {
    color: #2C2E32;
    -webkit-transition: all .3s;
    -moz-transition: all .3s;
    -ms-transition: all .3s;
    -o-transition: all .3s;
    transition: all .3s;
    opacity: .6;
    cursor: pointer;
    filter: drop-shadow(0 0 2px rgba(70, 70, 70, 0.8));
  }

  .action-icon:hover {
    opacity: 1;
    color: #2C2E32;
    filter: drop-shadow(0 0 8px rgba(0, 0, 0, 0.8));
  }

  i {
    padding-left: 8px;
    cursor: pointer;
  }

  i:hover {
    color: #fff;
  }

  svg {
    cursor: pointer;
    margin-top: 2px;
  }

  svg:hover {
    color: #fff;
  }

</style>
