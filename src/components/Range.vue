<template>
    <div class="wrap">
        <div class="cur-perc">{{modelValue}}%</div>
        <div class="range">
            <div class="inner" ref="rangeLine" @touchmove="moveThumbM" @click="clickRangeArea">
                <span class="thumb" :style="{'left': position + 'px'}" @mousedown="dragThumb" @mouseup="leaveThumb" @touchstart="dragThumb" @touchend="leaveThumb"></span>
            </div>
        </div>
        <ul>
            <li v-for="position in positionItem" :key="position.id">
                <span @click="moveThumbClick(position)">{{position}}</span>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: 'Range',
        props: {
            modelValue: Number
        },
        data() {
            return {
                position: this.modelValue,
                isDownThumb: false,
                positionItem: [25, 50, 75, 100]
            }
        },
        methods: {
            calcRangeWidth() {
                return this.$refs.rangeLine.offsetWidth - 20;
            },
            dragThumb(e) {
                this.isDownThumb = true;

                e.preventDefault();
            },
            leaveThumb(e) {
                this.isDownThumb = true;

                e.preventDefault();
            },
            moveThumbClick(x, e) {
                this.position = (this.calcRangeWidth()) * x / 100;

                this.$emit("update:modelValue", position);

                e.preventDefault();
            },
            clickRangeArea(e) {
                let bounds = this.$refs.rangeLine.getBoundingClientRect();
                let pageCoord = e.pageX;
                let x = pageCoord - bounds.left;
                let positionThumb;

                this.position = x - 10;

                if (x < 10) {
                    this.position = 0;
                }

                if (x > this.calcRangeWidth() - 20) {
                    this.position = x - 10;
                }

                positionThumb = (x - 10) / this.calcRangeWidth();
                positionThumb *= 100;

                if (positionThumb < 0) {
                    positionThumb = 0;
                }

                if (positionThumb > 100) {
                    positionThumb = 100;
                    this.position = this.calcRangeWidth();
                }

                this.$emit("update:modelValue", Math.round(positionThumb));
            }
        },
        mounted() {
            if (this.modelValue > 100 || this.modelValue < 0) {
                this.position = 0;
                this.$emit("update:modelValue", 0);
            } else {
                this.position = this.calcRangeWidth() * this.modelValue / 100;
                this.$emit("update:modelValue", this.modelValue);
            }

            document.addEventListener('mouseup', e => {
                this.isDownThumb = false;

                e.preventDefault();
            });

            document.addEventListener('touchend', e => {
                this.isDownThumb = false;

                e.preventDefault();
            });

            document.addEventListener('mousemove', e => {
                if (this.isDownThumb === true) {
                    let widthLine = this.calcRangeWidth();

                    if (this.position <= widthLine) {
                        let positionThumb;
                        let bounds = this.$refs.rangeLine.getBoundingClientRect();
                        let pageCoord = e.pageX;
                        let x = pageCoord - bounds.left;
                        
                        positionThumb = (x - 10) / widthLine;
                        positionThumb *= 100;

                        this.$emit("update:modelValue", Math.round(positionThumb));

                        this.position = x - 10;

                        if (positionThumb > 100) {
                            this.$emit("update:modelValue", 100);
                        }
                    } else {
                        this.isDownThumb = false;
                        this.position = this.calcRangeWidth();
                    }

                    if (this.position <= 0) {
                        this.isDownThumb = false;
                        this.position = 0;
                        this.$emit("update:modelValue", 0);
                    }
                }

                e.preventDefault();
            });

            document.addEventListener('touchmove', e => {
                if (this.isDownThumb === true) {
                    let widthLine = this.calcRangeWidth();

                    if (this.position <= widthLine) {
                        let positionThumb;
                        let bounds = this.$refs.rangeLine.getBoundingClientRect();
                        let pageCoord = e.touches[0].pageX;
                        let x = pageCoord - bounds.left;

                        positionThumb = x / widthLine;
                        positionThumb *= 100;

                        this.$emit("update:modelValue", Math.round(positionThumb));

                        this.position = x;

                        if (positionThumb > 100) {
                            this.$emit("update:modelValue", 100);
                        }
                    } else {
                        this.isDownThumb = false;
                        this.position = this.calcRangeWidth();
                    }

                    if (this.position <= 0) {
                        this.isDownThumb = false;
                        this.position = 0;
                        this.$emit("update:modelValue", 0);
                    }
                }

                e.preventDefault();
            });
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
        cursor: pointer;
    }

    ul {
        display: flex;
        padding: 0;
        margin-top: 20px;

        li {
            margin-right: 10px;
            list-style: none;

            span {
                display: block;
                padding: 5px;
                color: #fff;
                font-weight: bold;
                text-decoration: none;
                background: #ff4c4c;
                border-radius: 20px;
                cursor: pointer;
            }
        }
    }
</style>