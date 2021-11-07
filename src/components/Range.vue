<template>
    <div class="wrap">
        <div class="cur-perc">{{modelValue}}%</div>
        <div class="range">
            <div class="inner" ref="rangeLine" @mousemove="moveThumb" @touchmove="moveThumbM">
                <a href="#" class="thumb" :style="{'left': step + 'px'}" @mousedown="dragThumb" @mouseup="leaveThumb" @touchstart="dragThumb" @touchend="leaveThumb"></a>
            </div>
        </div>
        <ul>
            <li v-for="step in steps" :key="step.id">
                <a href="#" @click="moveThumbClick(step)">{{step}}</a>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: 'Range',
        props: {
            startValue: Number,
            modelValue: Number
        },
        data() {
            return {
                step: this.startValue,
                isDownThumb: false,
                data: 0,
                steps: [25, 50, 75, 100]
            }
        },
        methods: {
            dragThumb(e) {
                this.isDownThumb = true;

                e.preventDefault();
            },
            leaveThumb(e) {
                this.isDownThumb = false;

                e.preventDefault();
            },
            moveThumbClick(step, e) {
                this.step = (this.$refs.rangeLine.offsetWidth - 20) * step / 100;

                this.$emit("update:modelValue", step);

                e.preventDefault();
            },
            moveThumb(e) {
                if (this.isDownThumb === true) {
                    let widthLine = this.$refs.rangeLine.offsetWidth - 20;

                    if (this.step <= widthLine) {
                        let positionThumb;
                        let bounds = this.$refs.rangeLine.getBoundingClientRect();
                        let pageCoord = e.pageX;
                        let x = pageCoord - bounds.left;
                        
                        positionThumb = (x - 10) / widthLine;
                        positionThumb *= 100;

                        this.$emit("update:modelValue", Math.round(positionThumb));

                        this.step = x - 10;

                        if (positionThumb > 100) {
                            this.$emit("update:modelValue", 100);
                        }
                    } else {
                        this.isDownThumb = false;
                        this.step = this.$refs.rangeLine.offsetWidth - 20;
                    }

                    if (this.step <= 0) {
                        this.isDownThumb = false;
                        this.step = 0;
                        this.$emit("update:modelValue", 0);
                    }
                }

                e.preventDefault();
            },
            moveThumbM(e) {
                if (this.isDownThumb === true) {
                    let widthLine = this.$refs.rangeLine.offsetWidth - 20;

                    if (this.step <= widthLine) {
                        let positionThumb;
                        let bounds = this.$refs.rangeLine.getBoundingClientRect();
                        let pageCoord = e.touches[0].pageX;
                        let x = pageCoord - bounds.left;

                        positionThumb = x / widthLine;
                        positionThumb *= 100;

                        this.$emit("update:modelValue", Math.round(positionThumb));

                        this.step = x;

                        if (positionThumb > 100) {
                            this.$emit("update:modelValue", 100);
                        }
                    } else {
                        this.isDownThumb = false;
                        this.step = this.$refs.rangeLine.offsetWidth - 20;
                    }

                    if (this.step <= 0) {
                        this.isDownThumb = false;
                        this.step = 0;
                        this.$emit("update:modelValue", 0);
                    }
                }

                e.preventDefault();
            }
        },
        mounted() {
            if (this.startValue > 100 || this.startValue < 0) {
                this.step = 0;
                this.$emit("update:modelValue", 0);
            } else {
                
                this.step = (this.$refs.rangeLine.offsetWidth - 20) * this.startValue / 100;

                this.$emit("update:modelValue", this.startValue);
            }
        }
    }
</script>

<style lang="scss" scoped>
    .range {
        position: relative;
        width: 100%;
        height: 30px;
        display: flex;
        align-items: center;
        background: #0099e5;
        border-radius: 30px;
    }

    .inner {
        position: relative;
        width: 100%;
        height: 100%;
    }

    .thumb {
        position: absolute;
        top: 50%;
        left: 0;
        display: block;
        width: 20px;
        height: 20px;
        margin-top: -10px;
        background: #fff;
        border-radius: 100%;
    }

    ul {
        display: flex;
        padding: 0;
        margin-top: 20px;

        li {
            margin-right: 10px;
            list-style: none;

            a {
                display: block;
                padding: 5px;
                color: #fff;
                font-weight: bold;
                text-decoration: none;
                background: #ff4c4c;
                border-radius: 20px;
            }
        }
    }
</style>