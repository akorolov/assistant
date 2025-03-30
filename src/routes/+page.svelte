<script lang="ts">

import StoryModal from "$lib/StoryModal.svelte";
import { modals } from 'svelte-modals'
import { getContext, onMount, onDestroy } from 'svelte';
import { type ToastContext } from '@skeletonlabs/skeleton-svelte';

export const toast: ToastContext = getContext('toast');

let game_status = {
    "intro_story": true,
    "mana_earned": 0,
    "mana_spent": 0,
    "fire_earned": 0,
    "fire_spent": 0,
    "cultivator_name": "Radigen Banespell",
    "status": "Get back to work!",
    "rank": "Wood",
    "level": 0,
    "cultivate_add": 1,
    "fire_add": 1,
    "mana_increment": 0,
    "compress_cost": 100,
    "fire_compress": 0,
    "fire_compress_cost": 50
}

let autoIncrementerInterval: ReturnType<typeof setInterval>;

onMount(() => {
    autoIncrementerInterval = setInterval(() => {
        game_status.mana_earned += game_status.mana_increment;
        checkManaLevels();
    }, 1000); // Runs every half second
});

onDestroy(() => {
    if (autoIncrementerInterval) clearInterval(autoIncrementerInterval);
});

function triggerInfo(description: string) {
    toast.create({ description: description });
}

function checkManaLevels() {
    if (game_status.mana_earned >= 1000 && game_status.level == 2) {
        game_status.level = 3;
        modals.open(StoryModal, {title:"The Painful Road to Greatness", message: game_status.cultivator_name + " is not a patient master. Still, it could be worse. Remember the one who thought bathing would wash away his power? Yeah, you definitely didn't bat an eye when the firebirds ate him. Speaking of fire... Maybe you should try converting some of your mana."})
    }
}

if (game_status.intro_story) {
    modals.open(StoryModal,{title: "Assistant to the Cultivator", message: "You are an assistant to a development cultivator. All of your previous bosses died, but don't worry about that now! You're sure this will be the one that you help achieve greatness."})
    game_status.intro_story = false
}

function Cultivate() {
    game_status.mana_earned += game_status.cultivate_add;
    triggerInfo("+" + game_status.cultivate_add.toString() + " 🌀")
    if (game_status.mana_earned >= 10 && game_status.level == 0) {
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
        modals.open(StoryModal, {title: "The Long Road to Greatness", message: "You've cultivated a lot of mana! Try compressing it to make it more powerful!"})
    }
}

function CultivateFire() {
    game_status.fire_earned += game_status.fire_add;
    game_status.mana_spent += game_status.fire_add;
    triggerInfo("+" + game_status.fire_add.toString() + " 🔥, -" + game_status.fire_add.toString() + " 🌀")
    if (game_status.fire_earned >= 50 && game_status.fire_compress == 0) {
        modals.open(StoryModal, {title: "The Fire of the Soul", message:"Fire mana works a little different than regular mana. Try compressing it and see what happens!"})
        game_status.fire_compress = 1;
    }
}

function CompressMana() {
    game_status.mana_spent += game_status.compress_cost;
    game_status.mana_increment += 1;
    game_status.compress_cost = Math.round(game_status.compress_cost * 1.2);
    triggerInfo("You compressed your cultivator's mana!")
}

function CompressFire() {
    game_status.fire_spent += game_status.fire_compress_cost;
    game_status.cultivate_add += game_status.fire_add;
    game_status.fire_add += 1;
    game_status.fire_compress_cost = Math.round(game_status.fire_compress_cost * 1.4)
    triggerInfo("You compressed your cultivator's fire mana!")
}

</script>

<div class="flex flex-col gap-2">
    {#if game_status.mana_earned > 0}
    <div class="flex flex-row gap-2 items-center justify-center">
        🌀: {game_status.mana_earned - game_status.mana_spent}
        {#if game_status.fire_earned > 0}
        🔥: {game_status.fire_earned - game_status.fire_spent}
        {/if}
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
        <div class=" flex flex-col gap-2">
            <div class="flex flex-row gap-2">
                <button class="btn preset-filled-secondary-500" onclick={Cultivate}>Cultivate {game_status.cultivate_add} Mana</button>
                {#if game_status.level >= 2}
                    <button class="btn preset-filled-secondary-500" disabled={game_status.mana_earned - game_status.mana_spent < game_status.compress_cost} onclick={CompressMana}>Compress Mana ({game_status.compress_cost})</button>
                {/if}
            </div>
            <div class="flex flex-row gap-2">
                {#if game_status.level >= 3}
                    <button class="btn preset-filled-secondary-500" disabled={game_status.mana_earned - game_status.mana_spent < game_status.fire_add} onclick={CultivateFire}>Convert {game_status.fire_add} Fire Mana</button>
                {/if}
                {#if game_status.fire_compress >= 1}
                    <button class="btn preset-filled-secondary-500" disabled={game_status.fire_earned - game_status.fire_spent < game_status.fire_compress_cost} onclick={CompressFire}>Compress {game_status.fire_compress_cost} Fire Mana</button>
                {/if}
            </div>
            
        </div>
    </div>


</div>
<div>
    {JSON.stringify(game_status, null, 2)}
    <button class="btn" onclick={() => {game_status.mana_earned += 50}}>add 50 mana</button>
</div>