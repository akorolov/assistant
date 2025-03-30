<script lang="ts">

import StoryModal from "$lib/StoryModal.svelte";
import { modals } from 'svelte-modals'
import { getContext } from 'svelte';
import { type ToastContext } from '@skeletonlabs/skeleton-svelte';

export const toast: ToastContext = getContext('toast');

let game_status = {
    "intro_story": true,
    "mana_earned": 0,
    "mana_spent": 0,
    "cultivator_name": "Radigen Banespell",
    "status": "Get back to work!",
    "rank": "Wood",
    "level": 0,
    "cultivate_add": 1,
    "fire_add": 1,
}

function triggerInfo(description: string) {
    toast.create({ description: description });
}

if (game_status.intro_story) {
    modals.open(StoryModal,{title: "Assistant to the Cultivator", message: "You are an assistant to a development cultivator. All of your previous bosses died, but don't worry about that now! You're sure this will be the one that you help achieve greatness."})
    game_status.intro_story = false
}

function Cultivate() {
    game_status.mana_earned += game_status.cultivate_add;
    triggerInfo("+" + game_status.cultivate_add.toString() + " 🌀")
    if (game_status.mana_earned == 10) {
        game_status.cultivate_add += 1;
        triggerInfo("Mana cultivation increased!");
        triggerInfo("Level increased!")
        game_status.level = 1;
        modals.open(StoryModal, {title: "The Long Road to Greatness", message: "Congratulations! You cultivated enough mana to raise the level of your cultivator! Keep gaining levels and eventually you'll earn a new rank!"})
    }
    if (game_status.mana_earned >= 100 && game_status.level == 1) {
        game_status.cultivate_add += 1;
        triggerInfo("Mana cultivation increased!");
        triggerInfo("Level increased!")
        game_status.level = 2;
        modals.open(StoryModal, {title: "The Long Road to Greatness", message: "You've cultivated a lot of mana! Try compressing it into a new type!'"})
    }
}

function CultivateFire() {

}

function CompressMana() {
    
}

</script>

<div class="flex flex-col gap-2">
    {#if game_status.mana_earned > 0}
    <div class="flex flex-row gap-2 items-center justify-center">
        🌀: {game_status.mana_earned - game_status.mana_spent}
    </div>
    <hr class="hr">
    {/if}
    <div class="flex flex-row gap-4">
        <div class="flex flex-col items-start gap-2 border-2 p-2 bg-primary-400 border-primary-950">
            <span><b>Current Cultivator:</b> {game_status.cultivator_name}</span>
            <span class="italic">{game_status.status}</span>
            <hr class="hr">
            <span><b>Rank:</b> {game_status.rank}</span>
            <span><b>Level:</b> {game_status.level}</span>
    
        </div>
        <div class=" flex flex-col">
            <div class="flex flex-row">
                <button class="btn preset-filled-secondary-500" onclick={Cultivate}>Cultivate {game_status.cultivate_add} Mana</button>
                {#if game_status.level >= 2}
                    <button class="btn preset-filled-secondary-500" onclick={CompressMana}>Compress Mana</button>
                {/if}
            </div>
            {#if game_status.level >= 3}
                <button class="btn preset-filled-secondary-500" onclick={CultivateFire}>Cultivate {game_status.fire_add} Fire Mana</button>
            {/if}
        </div>
    </div>
</div>