<template>
    <div>
        <div class="select-wrapper">
            <div
                    class="selected"
                    @click="onDropDown"
            >
                {{ isMultiselect
                ? selectedItems.map(item => ' ' + item.label).toString()
                : selectedString}}
            </div>
            <div class="close-drop-down" v-if="isDropDown" @click="onDropDown"></div>
            <ul class="drop-down" v-if="isDropDown">
                <li
                        v-for="(element, index) in arrayData"
                        :key="index"
                        @click="selectItem(element)"
                        :class="{active: (selectedString === element.label) || (selectedItems.includes(element))}"
                >
                    {{element.label}}
                </li>
            </ul>
        </div>
        <div class="close-drop-down" v-if="isDropDown" @click="onDropDown"></div>
    </div>
</template>

<script>
    export default {
        name: "CustomSelect",
        props: ['arrayData', 'isMultiselect', 'setValue'],
        data() {
            return {
                selectedString: '',
                selectedItems: [],
                isDropDown: false
            }
        },
        methods: {
            onDropDown() {
                this.isDropDown = !this.isDropDown
            },
            selectItem(element) {
                if (!this.isMultiselect) {
                    this.selectedString = element.label
                    this.setValue(element.value)
                    this.onDropDown()
                } else {
                    if(this.selectedItems.includes(element)){
                        this.selectedItems.splice(this.selectedItems.indexOf(element), 1)
                    }else {
                        this.selectedItems.push(element)
                    }
                    this.setValue(this.selectedItems)
                }
            }
        }
    }
</script>

<style lang="scss">
    $ulBgColor: #fff;
    $inputBorderColor: #c3c3c3;
    $darkColor: #000;
    .select-wrapper {

        position: relative;

        .selected {
            padding: 8px 10px;
            font-size: 16px;
            border: 1px solid $inputBorderColor;
            border-radius: 4px;
            width: 100%;
            box-sizing: border-box;
            height: 36px;
            cursor: pointer;

            &:after {
                content: '';
                display: inline-block;
                position: relative;
                width: 0;
                height: 0;
                opacity: .3;
                border-left: 4px solid transparent;
                border-right: 4px solid transparent;
                border-top: 7px solid $darkColor;
                transition: all .3s ease-out;
                float: right;
                margin-top: 5px;
                margin-right: 5px;
            }
        }

        .drop-down {
            margin: 0;
            padding: 0;
            position: absolute;
            border: 1px solid $inputBorderColor;
            border-radius: 4px;
            width: 100%;
            background: $ulBgColor;
            list-style: none;
            box-sizing: border-box;
            margin-top: -1px;
            z-index: 2;
            li {
                padding: 8px 10px;
                font-size: 16px;
                border-bottom: 1px solid $inputBorderColor;
                width: 100%;
                box-sizing: border-box;
                cursor: pointer;
                &:hover{
                    background-color: #ececec;
                }

                &.active {
                    background: #ececec;
                }

                &:last-child {
                    border: none;
                }
            }
        }
    }

    .close-drop-down {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        z-index: 1;
    }

</style>