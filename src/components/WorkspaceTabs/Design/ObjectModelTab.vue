<template>
    <div class="flex flex-col max-w-3xl mx-auto px-8 bg-white pt-4">
        <HintBox :message="hintMessage"></HintBox>
        <div class="flex w-full">
            <div class="flex-1 mr-2">           
                <div class="flex h-8 bg-blue text-white border text-center justify-center items-center text-sm">sketch</div>
                <code-editor
                    class="w-full bg-grey-lightest rounded p-2 text-sm border" 
                    v-model="sketch"
                    lang="json"
                    :placeholder="placeholder"
                ></code-editor>
                <div class="mt-1">
                    <button @click="addUserSystem()" :class="buttonStyle">+ user system</button>
                    <button @click="replaceWithSampleApp()" :class="buttonStyle">sample app</button>
                    <button @click="replaceWithHelpApp()" :class="buttonStyle">help</button>
                </div>             

            </div>
            <div class="flex-1 ml-2">
                <div class="flex h-8 bg-grey-lighter border text-center justify-center items-center text-sm">schema</div>
                <code-editor
                    class="w-full bg-grey-lightest rounded p-2 text-sm border" 
                    v-model="schema"
                    lang="json"
                    :showGutter="true"
                ></code-editor>
                <!--
                <div class="mt-1">
                    <button :class="buttonStyle">+ some action</button>
                </div>
                -->
            </div>
        </div>
    </div>  
</template>

<script>
    export default {
        data() {
            return {
                hintMessage: "Start by listing your models in the sketch window (left). You may adjust the attribute properties in the schema window (right)",
                buttonStyle: "ml-2 text-xs border p-1 rounded shadow bg-white text-black px-2",
                // How to make a multiline placeholder?
                placeholder: "Start typing here. Click the buttons below to get some help on the syntax. "
            }
        },

        computed: {
            sketch: {
                get() {
                    return this.$store.state.sketch            
                },

                set(value) {
                    this.$store.dispatch('setSketch', value)
                }
            },

            schema: {
                get() {
                    return JSON.stringify(this.$store.state.schema, null, 4)
                },

                set(value) {
                    try {
                        let data = JSON.parse(value)
                        this.$store.dispatch('setSchema', data)
                    } catch(SyntaxError) {
                        // await fix by user
                    }
                }
            }
                                
        },

        methods: {
            addUserSystem() {
                this.$store.dispatch('setSketch', 
                    this.sketch + this.$store.state.masterFileFactory.userSystemSketch()
                )
            },

            replaceWithSampleApp() {
                this.$store.dispatch('setSketch', 
                    this.$store.state.masterFileFactory.sampleApp()
                )
            },
            
            replaceWithHelpApp() {
                this.$store.dispatch('setSketch', 
                    this.$store.state.masterFileFactory.helpApp()
                )
            },            
        }
    }
</script>