<template>
    <teleport to="#modal-root">
        <transition
            name="modal"
            @before-enter="isAnimating = true"
            @after-leave="isAnimating = false"
        >
            <div
                class="modal-mask"
                v-show="modelValue"
                v-on:pointerdown.self="close"
                v-bind="$attrs"
            >
                <div class="modal-wrapper">
                    <div class="modal-container">
                        <div class="modal-header">
                            <slot name="header" :shown="isOpen"> default header </slot>
                        </div>
                        <div class="modal-body">
                            <Links v-if="links" :links="links">
                                <slot name="body" :shown="isOpen"> default body </slot>
                            </Links>
                            <slot name="body" v-else :shown="isOpen"> default body </slot>
                        </div>
                        <div class="modal-footer">
                            <slot name="footer" :shown="isOpen">
                                <div class="modal-default-footer">
                                    <div class="modal-default-flex-grow"></div>
                                    <button class="button modal-default-button" @click="close">
                                        Close
                                    </button>
                                </div>
                            </slot>
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </teleport>
</template>

<script setup lang="ts">
import { Link } from "features/links";
import { computed, ref, toRefs } from "vue";
import Links from "./links/Links.vue";

const _props = defineProps<{
    modelValue: boolean;
    links?: Link[];
}>();
const props = toRefs(_props);
const emit = defineEmits<{
    (e: "update:modelValue", value: boolean): void;
}>();

const isOpen = computed(() => props.modelValue || isAnimating.value);
function close() {
    emit("update:modelValue", false);
}

const isAnimating = ref(false);

defineExpose({ isOpen });
</script>

<style>
.modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.5);
    transition: opacity 0.3s ease;
}

.modal-wrapper {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}

.modal-container {
    width: 640px;
    max-width: 95vw;
    max-height: 95vh;
    background-color: var(--background);
    padding: 20px;
    border-radius: 5px;
    transition: all 0.3s ease;
    text-align: left;
    border: var(--modal-border);
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
}

.modal-header {
    width: 100%;
}

.modal-body {
    margin: 20px 0;
    width: 100%;
    overflow-y: auto;
    overflow-x: hidden;
}

.modal-footer {
    width: 100%;
}

.modal-default-footer {
    display: flex;
}

.modal-default-flex-grow {
    flex-grow: 1;
}

.modal-enter-from {
    opacity: 0;
}

.modal-leave-active {
    opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-active .modal-container {
    -webkit-transform: scale(1.1);
    transform: scale(1.1);
}
</style>
