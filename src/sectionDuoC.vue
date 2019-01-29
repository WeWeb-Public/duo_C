<template>
    <div class="duo_C container-fluid">
        <!-- wwManager:start -->
        <wwSectionEditMenu v-bind:sectionCtrl="sectionCtrl"></wwSectionEditMenu>
        <!-- wwManager:end -->

        <wwObject class="background" v-bind:ww-object="section.data.background" ww-category="background"></wwObject>

        <div class="container section-padding">
            <div class="row">
                <div class="title-container">
                    <h1 class="section-title">
                        <wwObject v-bind:ww-object="section.data.title"></wwObject>
                    </h1>
                    <h2 class="section-subtitle">
                        <wwObject v-bind:ww-object="section.data.subtitle"></wwObject>
                    </h2>
                </div>
            </div>

            <div>
                <div class="row block-container" :class="{'one-block': section.data.blocks.length === 1}" v-for="(block, index) in section.data.blocks" :key="block.uniqueId">
                    <!-- wwManager:start -->
                    <div v-show="editMode" class="edit-button-top-left" @click="remove(section.data.blocks, { index })">
                        <i class="wwi wwi-delete" aria-hidden="true"></i>
                    </div>
                    <!-- wwManager:end -->

                    <div class="block-img-container block" :class="{'left col-sm-offset-1 col-md-offset-1': index % 2 === 0, 'right col-sm-push-6 col-md-push-6': index % 2 === 1}">
                        <div class="block-img">
                            <wwObject v-bind:ww-object="block.img"></wwObject>
                        </div>
                    </div>

                    <div class="block-content-container block" :class="{'left col-sm-offset-1 col-md-offset-1': index % 2 === 1, 'right col-sm-pull-5 col-md-pull-5': index % 2 === 1}">
                        <wwObject class="block-title" v-bind:ww-object="block.title"></wwObject>
                        <wwObject class="block-text" v-bind:ww-object="block.text"></wwObject>
                        <wwObject class="block-text" v-bind:ww-object="block.text2"></wwObject>
                        <wwObject class="block-text" v-bind:ww-object="block.text3"></wwObject>
                        <div class="button-container">
                            <wwObject class="button-wrapper" v-bind:ww-object="block.button"></wwObject>
                        </div>
                    </div>

                </div>

                <!-- wwManager:start -->
                <div class="row block-add-btn-container" v-show="editMode">
                    <wwLayoutPlus @click="add(section.data.blocks, getNewBlock())"></wwLayoutPlus>
                </div>
                <!-- wwManager:end -->
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "duo_C",
    props: {
        sectionCtrl: Object
    },
    data() {
        return {}
    },
    computed: {
        section() {
            return this.sectionCtrl.get();
        },
        // wwManager:start
        editMode() {
            return this.sectionCtrl.getEditMode() == 'CONTENT'
        }
        // wwManager:end
    },
    created() {
       //Initialize section data
        this.section.data = this.section.data || {};
        this.section.data.blocks = this.section.data.blocks || [this.getNewBlock()];

        //Initialize background
        if (!this.section.data.background) {
            this.section.data.background = wwLib.wwObject.getDefault({ type: 'ww-color' });
        }
        if (!this.section.data.title) {
            this.section.data.title = wwLib.wwObject.getDefault({ type: 'ww-text' });
        }
        if (!this.section.data.subtitle) {
            this.section.data.subtitle = wwLib.wwObject.getDefault({ type: 'ww-text' });
        }

        this.sectionCtrl.update(this.section);
    },
    methods: {
        getNewBlock() {
            return {
                img: wwLib.wwObject.getDefault({ type: 'ww-image' }),
                title: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                text: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                text2: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                text3: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                button: wwLib.wwObject.getDefault({ type: 'ww-button' })
            }
        },
        // wwManager:start
        add(array, elem) {
            array.push(elem);
            this.sectionCtrl.update(this.section);
        },
        remove(array, options) {
            array.splice(options.index, 1);
            this.sectionCtrl.update(this.section);
        }
        // wwManager:end
    }
};
</script>

<style lang="scss" scoped>
.duo_C .container {
    width: 100%;
    position: relative;
}

.duo_C .background {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
}

.duo_C .section-title {
    margin-bottom: 10px;
}

.duo_C .section-subtitle {
    margin-top: 10px;
    margin-bottom: 60px;
}

.duo_C .block-container {
    margin-bottom: 10px;
}

.duo_C .block {
    margin-bottom: 20px;
}

.duo_C .block-img {
    width: 100%;
}

.duo_C .block-title {
    margin-top: 10px;
}

.duo_C .block-text {
    margin-top: 10px;
}

.duo_C .button-container {
    text-align: center;
    width: 100%;
    margin-top: 20px;
    margin-bottom: 20px;
}

.duo_C .one-block {
    margin-bottom: 20px !important;
}

.duo_C .row {
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    margin-right: -15px;
    margin-left: -15px;
}

.duo_C .title-container {
    position: relative;
    width: 100%;
    min-height: 1px;
    padding-right: 15px;
    padding-left: 15px;
    -ms-flex: 0 0 100%;
    flex: 0 0 100%;
    max-width: 100%;
    margin-bottom: 20px;
}

.duo_C .container-fluid {
    padding-right: 15px;
    padding-left: 15px;
    margin-right: auto;
    margin-left: auto;
}

.duo_C .section-padding {
    padding: 30px 15px
}

.duo_C .block-img-container {
    position: relative;
    width: 100%;
    min-height: 1px;
    padding-right: 15px;
    padding-left: 15px;
    -ms-flex: 0 0 100%;
    flex: 0 0 100%;
    max-width: 100%;
}

.duo_C .block-content-container {
    position: relative;
    width: 100%;
    min-height: 1px;
    padding-right: 15px;
    padding-left: 15px;
    -ms-flex: 0 0 100%;
    flex: 0 0 100%;
    max-width: 100%;
}


@media (min-width: 768px) {
    .duo_C .block-container {
        margin-bottom: 60px;
    }
    .duo_C .block-img-container {
        -ms-flex: 0 0 41.666666%;
        flex: 0 0 41.666666%;
        max-width: 41.666666%;
        &.right {
            left: 50%;
        }
        &.left {
            margin-left: 8.333333%;
        }
    }
    .duo_C .block-content-container {
        -ms-flex: 0 0 41.666666%;
        flex: 0 0 41.666666%;
        max-width: 41.666666%;
        &.right {
            right: 41.666666%;
        }
        &.left {
            margin-left: 8.333333%;
        }
    }
    .duo_C .section-padding {
        padding: 50px 30px
    }
    .duo_C .block-container {
        position: relative;
        margin-bottom: 60px;
    }
}

@media (min-width: 992px) {
    .duo_C .section-padding {
        padding: 75px 50px
    }
}

@media (min-width: 1200px) {}

/* wwManager:start */
.duo_C .edit-button-top-left {
    position: absolute;
    left: 3px;
    top: -17px;
    width: 26px;
    height: 26px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 100%;
    text-align: center;
    font-size: 18px;
    line-height: 36px;
    cursor: pointer;
    pointer-events: all;
    z-index: 3;
    color: white;
    background-color: #E73055;
    background: linear-gradient(to right, #E73055 0%, rgb(175, 33, 61) 100%);
}

.duo_C .block-add-btn-container {
    display: flex;
    justify-content: center;
}
/* wwManager:end */
</style>
