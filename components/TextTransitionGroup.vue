<template>
    <p v-if="!done" class="text-transition-group">
        <span v-for="(word, i) in words" :key="i">{{ word }}</span>
    </p>
    <p v-else class="text-transition-group">
        <span v-for="(line, i) in lines" :key="i">
            {{ line.join('') }}
        </span>
    </p>

</template>

<script>
// Need to transform text content to wrapper with spans for each new line containing the text (on the new line)
// text should be passed as prop and wrapper element should be set to default as p
// text is split by each word and placed word by word, always checking for a line break (.getBoundingClientRect().top changes for the span)
export default {
    props: ['text', 'lineHeight', 'delay', 'stagger'],
    computed: {
        words(){ return this.text.split(/(\s+)/) }
    },
    data: () => ({
        lines: [],
        done: false
    }),
    mounted(){
        let lines = [], prevY, line = -1;
        Array.from(this.$el.children).forEach(span => {
            const y = span.getBoundingClientRect().top;

            if(typeof(prevY) === "undefined" || prevY !== y) line++;
            if(typeof(lines[line]) === "undefined") lines[line] = [];

            lines[line].push(span.innerHTML);
            prevY = y;
        })
        this.lines = lines;
        this.done = true;
    }
}
</script>