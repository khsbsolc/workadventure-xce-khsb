<script lang="ts">
    import { gameManager } from "../../Phaser/Game/GameManager";
    import { NETIQUETTE_URL, OPID_CLIENT_LOGOUT_URL } from "../../Enum/EnvironmentVariable";
    import { SelectCompanionScene, SelectCompanionSceneName } from "../../Phaser/Login/SelectCompanionScene";
    import { menuIconVisiblilityStore, menuVisiblilityStore } from "../../Stores/MenuStore";
    import { selectCompanionSceneVisibleStore } from "../../Stores/SelectCompanionStore";
    import { LoginScene, LoginSceneName } from "../../Phaser/Login/LoginScene";
    import { loginSceneVisibleStore } from "../../Stores/LoginSceneStore";
    import { selectCharacterSceneVisibleStore } from "../../Stores/SelectCharacterStore";
    import { SelectCharacterScene, SelectCharacterSceneName } from "../../Phaser/Login/SelectCharacterScene";
    import { EnableCameraScene, EnableCameraSceneName } from "../../Phaser/Login/EnableCameraScene";
    import { enableCameraSceneVisibilityStore } from "../../Stores/MediaStore";
    import btnProfileSubMenuCamera from "../images/btn-menu-profile-camera.svg";
    import btnProfileSubMenuIdentity from "../images/btn-menu-profile-identity.svg";
    import btnProfileSubMenuLogout from "../images/btn-menu-profile-logout.svg";
    import btnProfileSubMenuCompanion from "../images/btn-menu-profile-companion.svg";
    import btnProfileSubMenuNetiquette from "../images/btn-menu-profile-netiquette.svg";
    import Woka from "../Woka/Woka.svelte";
    import Companion from "../Companion/Companion.svelte";
    import LL from "../../i18n/i18n-svelte";
    import { analyticsClient } from "../../Administration/AnalyticsClient";

    function disableMenuStores() {
        menuVisiblilityStore.set(false);
        menuIconVisiblilityStore.set(false);
    }

    function openEditCompanionScene() {
        disableMenuStores();
        selectCompanionSceneVisibleStore.set(true);
        gameManager.leaveGame(SelectCompanionSceneName, new SelectCompanionScene());
    }

    function openEditNameScene() {
        disableMenuStores();
        loginSceneVisibleStore.set(true);
        gameManager.leaveGame(LoginSceneName, new LoginScene());
    }

    function openEditSkinScene() {
        disableMenuStores();
        selectCharacterSceneVisibleStore.set(true);
        gameManager.leaveGame(SelectCharacterSceneName, new SelectCharacterScene());
    }

    function openEnableCameraScene() {
        disableMenuStores();
        enableCameraSceneVisibilityStore.showEnableCameraScene();
        gameManager.leaveGame(EnableCameraSceneName, new EnableCameraScene());
    }

    function openNetiquettte() {
        window.open(NETIQUETTE_URL, "_blank");
    }

    function logout() {
        window.open(OPID_CLIENT_LOGOUT_URL, "_self");
    }
</script>

<div class="customize-main">
    <div class="submenu">
        <section class="centered-column resizing-width tw-m-auto resizing-text">
            <button
                type="button"
                class="tw-w-full outline"
                on:click={() => analyticsClient.editName()}
                on:click={openEditNameScene}
            >
                <img
                    src={btnProfileSubMenuIdentity}
                    alt={$LL.menu.profile.edit.name()}
                    width="26px"
                    height="26px"
                    class="darken-icon"
                />
                <span class="">{$LL.menu.profile.edit.name()}</span>
            </button>
            <button
                type="button"
                class="tw-w-full outline"
                on:click={() => analyticsClient.editWoka()}
                on:click={openEditSkinScene}
            >
                <Woka userId={-1} placeholderSrc="" width="26px" height="26px" />
                <span class="">{$LL.menu.profile.edit.woka()}</span>
            </button>
            <button
                type="button"
                class="tw-w-full outline"
                on:click={() => analyticsClient.editCompanion()}
                on:click={openEditCompanionScene}
            >
                <Companion userId={-1} placeholderSrc={btnProfileSubMenuCompanion} width="26px" height="26px" />
                <span class="">{$LL.menu.profile.edit.companion()}</span>
            </button>
            <button
                type="button"
                class="tw-w-full outline"
                on:click={() => analyticsClient.editCamera()}
                on:click={openEnableCameraScene}
            >
                <img
                    src={btnProfileSubMenuCamera}
                    alt={$LL.menu.profile.edit.camera()}
                    width="26px"
                    height="26px"
                    class="darken-icon"
                />
                <span class="">{$LL.menu.profile.edit.camera()}</span>
            </button>
            {#if NETIQUETTE_URL}
            <button type="button" class="tw-w-full outline" on:click|preventDefault={openNetiquettte}>
                <img
                    src={btnProfileSubMenuNetiquette}
                    alt={$LL.menu.profile.netiquette()}
                    width="26px"
                    height="26px"
                />
                <span class="btn-hover">{$LL.menu.profile.netiquette()}</span>
            </button>
            {/if}
            {#if OPID_CLIENT_LOGOUT_URL}
                <button type="button" class="tw-w-full outline" on:click|preventDefault={logout}>
                    <img
                        src={btnProfileSubMenuLogout}
                        alt={$LL.menu.profile.logout()}
                        width="26px"
                        height="26px"
                    />
                    <span class="btn-hover">{$LL.menu.profile.logout()}</span>
                </button>
            {/if}
        </section>
    </div>

    <div class="content" />
</div>

<style lang="scss">
    @import "../../../style/breakpoints.scss";
</style>
