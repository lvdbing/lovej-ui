<script>
export default{
    props: {
        parentMsg: String
    },
    emits: ['response'],
    created() {
        this.$emit('response', 'Hello from child')
    }
}
</script>

<template>
    <h2>A Child Component!</h2>
    <h3>Parent msg: {{ parentMsg || 'No props passed yet' }}</h3>
    <slot>Fallback content</slot>
</template>