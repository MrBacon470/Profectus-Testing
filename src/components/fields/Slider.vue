<template>
    <div class="field">
        <span class="field-title" v-if="title">{{ title }}</span>
        <Tooltip :display="`${value}`" :class="{ fullWidth: !title }">
            <input type="range" v-model="value" :min="min" :max="max" />
        </Tooltip>
    </div>
</template>

<script setup lang="ts">
import { computed, toRefs, unref } from "vue";
import Tooltip from "../Tooltip.vue";
import "components/common/fields.css";

const _props = defineProps<{
    title?: string;
    modelValue?: number;
    min?: number;
    max?: number;
}>();
const props = toRefs(_props);
const emit = defineEmits<{
    (e: "update:modelValue", value: number): void;
}>();

const value = computed({
    get() {
        return String(unref(props.modelValue) || 0);
    },
    set(value: string) {
        emit("update:modelValue", Number(value));
    }
});
</script>

<style scoped>
.fullWidth {
    width: 100%;
}
</style>
