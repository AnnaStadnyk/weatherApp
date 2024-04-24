<template>
    <div class="flex flex-col gap-[2px] items-center text-base">
        <div class="text-lg"><slot name="img"></slot></div>
        <slot></slot>
        <div class="text-sm">
            <slot name="description"></slot>
        </div>
    </div>
</template>