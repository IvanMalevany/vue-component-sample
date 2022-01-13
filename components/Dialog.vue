<template>
    <div
            class="popup"
            ref="dialogWindow"
            :class="{'popup_active':open}"
            v-if="open"
            @click="preClose"
    >
        <div class="popup__wrapper popup__wrapper_full-height">
            <div class="dialog">
                <svg
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 341.333 341.333"
                        class="dialog__close"
                        @click="close"
                >
                    <g><polygon points="341.333,149.333 81.707,149.333 200.853,30.187 170.667,0 0,170.667 170.667,341.333 200.853,311.147 81.707,192     341.333,192   " fill="#0288d1" data-original="#000000" style="" class=""/></g>
                </svg>
                <svg
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 459 459"
                        class="dialog__menu"
                        @click="controlsOpen = true"
                >
                    <g><g><g><path d="M0,382.5h459v-51H0V382.5z M0,255h459v-51H0V255z M0,76.5v51h459v-51H0z" data-original="#000000" class="active-path" fill="#000000"/></g></g></g>
                </svg>
                <div class="dialog__top">
                    <div class="dialog__client">
                        <span
                                class="bold"
                                @click="openContacts"
                        >
                            {{ name }}
                            <span
                                    class="dialog__client-contacts"
                                    :class="{'dialog__client-contacts_open':contactsView}"
                            >
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M11,9H13V7H11M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2M11,17H13V11H11V17Z" /></svg>
                            </span>
                        </span>
                    </div>
                    <div
                            class="dialog__contacts"
                            v-show="dialog.belong === 'user'"
                    >
                        <a
                                class="dialog__contact"
                                :href="'tel:'+dialog.client.phone"
                                v-show="dialog.client.phone"
                        >
                            <span><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 401.998 401.998"><g><path d="M401.129,311.475c-1.137-3.426-8.371-8.473-21.697-15.129c-3.61-2.098-8.754-4.949-15.41-8.566   c-6.662-3.617-12.709-6.95-18.13-9.996c-5.432-3.045-10.521-5.995-15.276-8.846c-0.76-0.571-3.139-2.234-7.136-5   c-4.001-2.758-7.375-4.805-10.14-6.14c-2.759-1.327-5.473-1.995-8.138-1.995c-3.806,0-8.56,2.714-14.268,8.135   c-5.708,5.428-10.944,11.324-15.7,17.706c-4.757,6.379-9.802,12.275-15.126,17.7c-5.332,5.427-9.713,8.138-13.135,8.138   c-1.718,0-3.86-0.479-6.427-1.424c-2.566-0.951-4.518-1.766-5.858-2.423c-1.328-0.671-3.607-1.999-6.845-4.004   c-3.244-1.999-5.048-3.094-5.428-3.285c-26.075-14.469-48.438-31.029-67.093-49.676c-18.649-18.658-35.211-41.019-49.676-67.097   c-0.19-0.381-1.287-2.19-3.284-5.424c-2-3.237-3.333-5.518-3.999-6.854c-0.666-1.331-1.475-3.283-2.425-5.852   s-1.427-4.709-1.427-6.424c0-3.424,2.713-7.804,8.138-13.134c5.424-5.327,11.326-10.373,17.7-15.128   c6.379-4.755,12.275-9.991,17.701-15.699c5.424-5.711,8.136-10.467,8.136-14.273c0-2.663-0.666-5.378-1.997-8.137   c-1.332-2.765-3.378-6.139-6.139-10.138c-2.762-3.997-4.427-6.374-4.999-7.139c-2.852-4.755-5.799-9.846-8.848-15.271   c-3.049-5.424-6.377-11.47-9.995-18.131c-3.615-6.658-6.468-11.799-8.564-15.415C98.986,9.233,93.943,1.997,90.516,0.859   C89.183,0.288,87.183,0,84.521,0c-5.142,0-11.85,0.95-20.129,2.856c-8.282,1.903-14.799,3.899-19.558,5.996   c-9.517,3.995-19.604,15.605-30.264,34.826C4.863,61.566,0.01,79.271,0.01,96.78c0,5.135,0.333,10.131,0.999,14.989   c0.666,4.853,1.856,10.326,3.571,16.418c1.712,6.09,3.093,10.614,4.137,13.56c1.045,2.948,2.996,8.229,5.852,15.845   c2.852,7.614,4.567,12.275,5.138,13.988c6.661,18.654,14.56,35.307,23.695,49.964c15.03,24.362,35.541,49.539,61.521,75.521   c25.981,25.98,51.153,46.49,75.517,61.526c14.655,9.134,31.314,17.032,49.965,23.698c1.714,0.568,6.375,2.279,13.986,5.141   c7.614,2.854,12.897,4.805,15.845,5.852c2.949,1.048,7.474,2.43,13.559,4.145c6.098,1.715,11.566,2.905,16.419,3.576   c4.856,0.657,9.853,0.996,14.989,0.996c17.508,0,35.214-4.856,53.105-14.562c19.219-10.656,30.826-20.745,34.823-30.269   c2.102-4.754,4.093-11.273,5.996-19.555c1.909-8.278,2.857-14.985,2.857-20.126C401.99,314.814,401.703,312.819,401.129,311.475z" fill="#424242"/></g></svg></span>
                            {{ dialog.client.phone }}
                        </a>
                        <a
                                class="dialog__contact"
                                :href="'mailto:'+dialog.client.email"
                                v-show="dialog.client.email"
                        >
                            <span><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 479.058 479.058"><g><path d="m434.146 59.882h-389.234c-24.766 0-44.912 20.146-44.912 44.912v269.47c0 24.766 20.146 44.912 44.912 44.912h389.234c24.766 0 44.912-20.146 44.912-44.912v-269.47c0-24.766-20.146-44.912-44.912-44.912zm0 29.941c2.034 0 3.969.422 5.738 1.159l-200.355 173.649-200.356-173.649c1.769-.736 3.704-1.159 5.738-1.159zm0 299.411h-389.234c-8.26 0-14.971-6.71-14.971-14.971v-251.648l199.778 173.141c2.822 2.441 6.316 3.655 9.81 3.655s6.988-1.213 9.81-3.655l199.778-173.141v251.649c-.001 8.26-6.711 14.97-14.971 14.97z" fill="#424242"/></g></svg></span>
                            {{ dialog.client.email }}
                        </a>
                        <div
                                class="dialog__contact"
                                v-show="dialog.client.address"
                        >
                            <span><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 510 510"><g><path d="M255,0C155.55,0,76.5,79.05,76.5,178.5C76.5,311.1,255,510,255,510s178.5-198.9,178.5-331.5C433.5,79.05,354.45,0,255,0z     M255,242.25c-35.7,0-63.75-28.05-63.75-63.75s28.05-63.75,63.75-63.75s63.75,28.05,63.75,63.75S290.7,242.25,255,242.25z" fill="#424242"/></g></svg></span>
                            {{ dialog.client.address }}
                        </div>
                    </div>
                </div>
                <div class="dialog__bg">
                    <div
                            class="dialog__contacts-view"
                            :class="{'dialog__contacts-view_open':contactsView}"
                    >
                        <a
                                :href="'tel:'+dialog.client.phone"
                                v-show="dialog.client.phone"
                        >
                            <div class="dialog__contacts-view-icon">
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 401.998 401.998" xml:space="preserve"><path d="M401.129,311.475c-1.137-3.426-8.371-8.473-21.697-15.129c-3.61-2.098-8.754-4.949-15.41-8.566   c-6.662-3.617-12.709-6.95-18.13-9.996c-5.432-3.045-10.521-5.995-15.276-8.846c-0.76-0.571-3.139-2.234-7.136-5   c-4.001-2.758-7.375-4.805-10.14-6.14c-2.759-1.327-5.473-1.995-8.138-1.995c-3.806,0-8.56,2.714-14.268,8.135   c-5.708,5.428-10.944,11.324-15.7,17.706c-4.757,6.379-9.802,12.275-15.126,17.7c-5.332,5.427-9.713,8.138-13.135,8.138   c-1.718,0-3.86-0.479-6.427-1.424c-2.566-0.951-4.518-1.766-5.858-2.423c-1.328-0.671-3.607-1.999-6.845-4.004   c-3.244-1.999-5.048-3.094-5.428-3.285c-26.075-14.469-48.438-31.029-67.093-49.676c-18.649-18.658-35.211-41.019-49.676-67.097   c-0.19-0.381-1.287-2.19-3.284-5.424c-2-3.237-3.333-5.518-3.999-6.854c-0.666-1.331-1.475-3.283-2.425-5.852   s-1.427-4.709-1.427-6.424c0-3.424,2.713-7.804,8.138-13.134c5.424-5.327,11.326-10.373,17.7-15.128   c6.379-4.755,12.275-9.991,17.701-15.699c5.424-5.711,8.136-10.467,8.136-14.273c0-2.663-0.666-5.378-1.997-8.137   c-1.332-2.765-3.378-6.139-6.139-10.138c-2.762-3.997-4.427-6.374-4.999-7.139c-2.852-4.755-5.799-9.846-8.848-15.271   c-3.049-5.424-6.377-11.47-9.995-18.131c-3.615-6.658-6.468-11.799-8.564-15.415C98.986,9.233,93.943,1.997,90.516,0.859   C89.183,0.288,87.183,0,84.521,0c-5.142,0-11.85,0.95-20.129,2.856c-8.282,1.903-14.799,3.899-19.558,5.996   c-9.517,3.995-19.604,15.605-30.264,34.826C4.863,61.566,0.01,79.271,0.01,96.78c0,5.135,0.333,10.131,0.999,14.989   c0.666,4.853,1.856,10.326,3.571,16.418c1.712,6.09,3.093,10.614,4.137,13.56c1.045,2.948,2.996,8.229,5.852,15.845   c2.852,7.614,4.567,12.275,5.138,13.988c6.661,18.654,14.56,35.307,23.695,49.964c15.03,24.362,35.541,49.539,61.521,75.521   c25.981,25.98,51.153,46.49,75.517,61.526c14.655,9.134,31.314,17.032,49.965,23.698c1.714,0.568,6.375,2.279,13.986,5.141   c7.614,2.854,12.897,4.805,15.845,5.852c2.949,1.048,7.474,2.43,13.559,4.145c6.098,1.715,11.566,2.905,16.419,3.576   c4.856,0.657,9.853,0.996,14.989,0.996c17.508,0,35.214-4.856,53.105-14.562c19.219-10.656,30.826-20.745,34.823-30.269   c2.102-4.754,4.093-11.273,5.996-19.555c1.909-8.278,2.857-14.985,2.857-20.126C401.99,314.814,401.703,312.819,401.129,311.475z" fill="#424242"/></svg>
                            </div>
                            <div class="dialog__contacts-view-text bold" v-text="dialog.client.phone"></div>
                        </a>
                        <a
                                :href="'mailto:'+dialog.client.email"
                                v-show="dialog.client.email"
                        >
                            <div class="dialog__contacts-view-icon">
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 479.058 479.058"><g><path d="m434.146 59.882h-389.234c-24.766 0-44.912 20.146-44.912 44.912v269.47c0 24.766 20.146 44.912 44.912 44.912h389.234c24.766 0 44.912-20.146 44.912-44.912v-269.47c0-24.766-20.146-44.912-44.912-44.912zm0 29.941c2.034 0 3.969.422 5.738 1.159l-200.355 173.649-200.356-173.649c1.769-.736 3.704-1.159 5.738-1.159zm0 299.411h-389.234c-8.26 0-14.971-6.71-14.971-14.971v-251.648l199.778 173.141c2.822 2.441 6.316 3.655 9.81 3.655s6.988-1.213 9.81-3.655l199.778-173.141v251.649c-.001 8.26-6.711 14.97-14.971 14.97z" fill="#424242"/></g> </svg>
                            </div>
                            <div class="dialog__contacts-view-text bold" v-text="dialog.client.email"></div>
                        </a>
                        <div v-show="dialog.client.address">
                            <div class="dialog__contacts-view-icon">
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 510 510" xml:space="preserve"><path d="M255,0C155.55,0,76.5,79.05,76.5,178.5C76.5,311.1,255,510,255,510s178.5-198.9,178.5-331.5C433.5,79.05,354.45,0,255,0z     M255,242.25c-35.7,0-63.75-28.05-63.75-63.75s28.05-63.75,63.75-63.75s63.75,28.05,63.75,63.75S290.7,242.25,255,242.25z" fill="#424242"/></svg>
                            </div>
                            <div class="dialog__contacts-view-text bold" v-text="dialog.client.address"></div>
                        </div>
                    </div>
                    <div
                            class="dialog__controls"
                            ref="dialogControls"
                            :class="{'dialog__controls_mobile-open':controlsOpen}"
                            @click="($event.target === $refs.dialogControls)? controlsOpen = false : null"
                    >
                        <span
                                v-show="!isBlocked && cancelOrTransfer"
                                v-text="$t('dialog.actions.cancel')"
                                @click="cancelDialog"
                        ></span>
                        <span
                                v-show="!isBlocked && cancelOrTransfer"
                                v-text="$t('dialog.actions.transfer')"
                                @click="passToEmployee"
                        ></span>
                        <span
                                v-show="!isBlocked"
                                v-text="$t('dialog.actions.block')"
                                @click="block"
                        ></span>
                        <span
                                v-show="isBlocked"
                                v-text="$t('dialog.actions.unblock')"
                                @click="unBlock"
                        ></span>
                        <span
                                v-text="$t('dialog.actions.addToCRM')"
                                @click="addToCRM"
                        ></span>
                        <span
                                v-text="$t('dialog.actions.toArchive')"
                                v-show="!isArchived"
                                @click="moveToArchive"
                        ></span>
                        <span
                                v-text="$t('dialog.actions.toDialogs')"
                                v-show="isArchived"
                                @click="moveToDialogs"
                        ></span>
                        <span
                                v-text="$t('dialog.actions.remove')"
                                @click="removeDialog"
                        ></span>
                    </div>
                    <div class="dialog__messages" ref="messages">

                        <Message v-for="(message, index) in messages" :key="index" :message="message" :dialog="dialog"/>

                        <div class="dialog__typing">
                            <div
                                    v-show="typingText"
                                    v-text="typingText"
                            ></div>
                        </div>

                    </div>
                    <div class="new-message-cover">
                        <div class="new-message-files">
                            <div
                                    class="new-message-files__item"
                                    v-for="(file, index) in files"
                                    :key="index"
                            >
                                <a
                                        target="_blank"
                                        :href="file.url"
                                        v-text="file.name"
                                ></a>
                                <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        viewBox="0 0 24 24"
                                        @click="removeFile(index)"
                                >
                                    <path d="M19,6.41L17.59,5L12,10.59L6.41,5L5,6.41L10.59,12L5,17.59L6.41,19L12,13.41L17.59,19L19,17.59L13.41,12L19,6.41Z"></path>
                                </svg>
                            </div>
                        </div>
                        <div class="new-message">
                            <span class="new-message__attach">
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 30.34 30.34"><path d="M22.562 12.491s1.227-.933.293-1.866c-.934-.933-1.842.271-1.842.271l-9.389 9.391s-2.199 2.838-3.871 1.122c-1.67-1.718 1.121-3.872 1.121-3.872l12.311-12.31s2.873-3.165 5.574-.466c2.697 2.7-.477 5.579-.477 5.579L12.449 24.173s-4.426 5.113-8.523 1.015 1.066-8.474 1.066-8.474L15.494 6.209s1.176-.982.295-1.866c-.885-.883-1.865.295-1.865.295L1.873 16.689s-4.549 4.989.531 10.068c5.08 5.082 10.072.533 10.072.533l16.563-16.565s3.314-3.655-.637-7.608-7.607-.639-7.607-.639L6.543 16.728s-3.65 2.969-.338 6.279c3.312 3.314 6.227-.39 6.227-.39l10.13-10.126z"></path></svg>
                                <input type="file" multiple @change="addFile"/>
                            </span>
                            <textarea
                                    class="new-message__input"
                                    :class="{'new-message__input_error':newMessageError}"
                                    :placeholder="$t('dialog.message')"
                                    v-model="newMessage"
                                    @keydown.prevent.enter="send"
                                    @input="typing"
                            ></textarea>
                            <span
                                    class="new-message__send"
                                    @click="send"
                            >
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512.001 512.001">
                                    <path d="M507.608,4.395c-4.243-4.244-10.609-5.549-16.177-3.321L9.43,193.872c-5.515,2.206-9.208,7.458-9.42,13.395
                                        c-0.211,5.936,3.101,11.437,8.445,14.029l190.068,92.181l92.182,190.068c2.514,5.184,7.764,8.455,13.493,8.455
                                        c0.178,0,0.357-0.003,0.536-0.01c5.935-0.211,11.189-3.904,13.394-9.419l192.8-481.998
                                        C513.156,15.001,511.851,8.638,507.608,4.395z M52.094,209.118L434.72,56.069L206.691,284.096L52.094,209.118z M302.883,459.907
                                        l-74.979-154.599l228.03-228.027L302.883,459.907z"/>
                                </svg>
                            </span>
                        </div>
                    </div>
                </div>
            </div>
            <div
                    class="popup__disabled"
                    :class="{'popup__disabled_active':loading}"
            ></div>
        </div>
    </div>
</template>

<script>
import { mapState, mapActions, mapGetters, mapMutations } from 'vuex'
import Message from "components/DialogMessage.vue"
import { axios } from 'boot/axios.js'

export default {
    name: 'Dialog',
    data(){
        return {
            dialog: null,
            controlsOpen: false,
            newMessage: '',
            newMessageError: false,
            files: [],
            loading: false,
            contactsView: false
        }
    },
    components: {
        Message
    },
    watch: {
        open(n){
            const vm = this;
            if(n){
                vm.dialog = vm.getData(n.id);
                if(n.activateFunction) {
                    vm[n.activateFunction]();
                    vm.changeWindowProps({
                        window: 'dialog',
                        props: {id: n.id}
                    });
                }
                this.$nextTick(() => this.scrollDown());
            }
        }
    },
    computed: {
        ...mapGetters({
            isWindowOpen: 'popup/isWindowOpen',
            getData: 'dialog/dialog',
            getEmployee: 'app/getEmployee',
            accountData: 'app/accountData',
            fileLimits: 'app/fileLimits',
            syncToken: 'app/syncToken'
        }),
        isArchived(){
            return this.dialog.type === 'archive';
        },
        isBlocked(){
            return this.dialog.block;
        },
        cancelOrTransfer(){
            const vm = this;
            if(vm.dialog.belong === 'user') return vm.dialog.employee === vm.accountData.id;
            else return false;
        },
        typingText(){
            const vm = this;
            let typing = vm.dialog.typing;
            if(typing.type) {
                return (typing.type === 'employee')?
                    vm.$t('dialog.typing.employee', {name: typing.name})
                    :
                    vm.$t('dialog.typing.client', {name: typing.name})
            }
            else return false;
        },
        open(){
            return this.isWindowOpen('dialog');
        },
        name(){
            const vm = this;
            return (vm.dialog.belong === 'user')? vm.dialog.client.name : [...this.dialog.team].map(e => vm.getEmployee(e).name).join(', ')
        },
        messages(){
            return [...this.dialog.messages].sort((m1, m2) => m1.date - m2.date);
        }
    },
    methods: {
        ...mapActions({
            checkExist: 'clients/checkClientExist',
            addMessage: 'dialog/addMessage',
            sendTypingSignal: 'dialog/sendTypingSignal',
            removeDialogAction: 'dialog/remove',
            cancelDialogAction: 'dialog/cancel',
            moveToArchiveAction: 'dialog/archive',
            moveToDialogsAction: 'dialog/unarchive',
            blockDialog: 'dialog/block',
            unblockDialog: 'dialog/unblock',
        }),
        ...mapMutations({
            openWindow: 'popup/open',
            closeWindow: 'popup/close',
            changeWindowProps: 'popup/changeWindowProps',
            removeDialogMutation: 'dialog/remove',
            removeFromDisk: 'app/removeFromDisk',
        }),
        preClose(e){
            if(e.target === this.$refs.dialogWindow) this.close();
        },
        close(){
            this.contactsView = false;
            this.clearFiles();
            this.closeWindow('dialog');
        },
        addFile(e){
            const vm = this;
            let errors = [];
            let files = e.target.files;
            if(files.length){
                for(let file of files){
                    if(vm.fileLimits.accept.includes(file.name.split('.').pop().toLowerCase())){
                        if(file.size <= vm.fileLimits.maxSize){
                            let reader = new FileReader();
                            reader.readAsDataURL(file);
                            reader.onload = () => {
                                vm.files.push({name: file.name, base64: reader.result, url: URL.createObjectURL(file), size: file.size});
                                this.typing();
                            };
                        }
                        else {
                            errors.push({
                                file: file.name,
                                error: 'MaxFileSize'
                            });
                        }
                    }
                    else {
                        errors.push({
                            file: file.name,
                            error: 'ImageTypeNotSupported'
                        });
                    }
                }
                if(errors.length){
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: errors.map(e => vm.$t('server.errors.FileUploadError', {
                                file: e.file,
                                error: vm.$t('server.errors.'+e.error)
                            })).join(', ')
                        }
                    })
                }
            }
        },
        send(){
            const vm = this;
            if(vm.newMessage.trim().length || vm.files.length){
                vm.addMessage({
                    dialogID: vm.dialog.id,
                    message: vm.newMessage.trim(),
                    attach: vm.files,
                })
                    .then(() => {
                        vm.newMessage = '';
                        vm.files = [];
                        vm.$nextTick(() => vm.scrollDown());
                    })
                    .catch(e => {
                        vm.openWindow({
                            name: 'alert',
                            props: {
                                message: vm.$t(`server.errors.${e}`)
                            }
                        })
                    })
            }
            else vm.newMessageError = true;
        },
        removeFile(index){
            URL.revokeObjectURL(this.files[index].url);
            this.files.splice(index, 1);
        },
        clearFiles(){
            let { files } = this;
            if(files.length){
                for(let file of files) URL.revokeObjectURL(file.url);
            }
        },
        scrollDown(){
            const vm = this;
            if(vm.$refs.messages) vm.$refs.messages.scrollTop = vm.$refs.messages.scrollHeight;
        },
        typing(){
            const vm = this;
            vm.newMessageError = false;
            vm.sendTypingSignal(vm.dialog.id);
        },
        removeDialog(){
            const vm = this;
            this.openWindow({name: 'confirm', props: {
                message: vm.$t('dialogs.removeOne'),
                confirm: () => {
                    vm.loading = true;
                    let dialogID = vm.dialog.id;
                    vm.removeDialogAction(dialogID)
                        .then(result => {
                            vm.removeFromDisk(result.data.data.free);
                            vm.close();
                            vm.removeDialogMutation(dialogID);
                        })
                        .catch(e => {
                            vm.openWindow({
                                name: 'alert',
                                props: {
                                    message: vm.$t(`server.errors.${e}`)
                                }
                            })
                        })
                        .then(() => vm.loading = false);
                }
            }});
        },
        cancelDialog(){
            const vm = this;
            vm.loading = true;
            vm.cancelDialogAction(vm.dialog.id)
                .then(() => {

                })
                .catch(e => {
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: vm.$t(`server.errors.${e}`)
                        }
                    })
                })
                .then(() => vm.loading = false);
        },
        moveToDialogs(){
            const vm = this;
            vm.loading = true;
            vm.moveToDialogsAction(vm.dialog.id)
                .then(() => {
                    vm.close();
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: vm.$t('dialogs.movedToDialogs')
                        }
                    })
                })
                .catch(e => {
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: vm.$t(`server.errors.${e}`)
                        }
                    })
                })
                .then(() => vm.loading = false);
        },
        moveToArchive(){
            const vm = this;
            vm.loading = true;
            vm.moveToArchiveAction(vm.dialog.id)
                .then(() => {
                    vm.close();
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: vm.$t('dialogs.movedToArchive')
                        }
                    })
                })
                .catch(e => {
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: vm.$t(`server.errors.${e}`)
                        }
                    })
                })
                .then(() => vm.loading = false);
        },
        passToEmployee(){
            this.openWindow({
                name: 'PassDialogToEmployee',
                props: {
                    dialog: this.dialog.id
                }
            })
        },
        block(){
            const vm = this;
            this.openWindow({name: 'confirm', props: {
                    message: vm.$t('dialogs.blockDialog'),
                    confirm: () => {
                        vm.loading = true;
                        vm.blockDialog(vm.dialog.id)
                            .then(() => {
                                vm.close();
                                vm.openWindow({
                                    name: 'alert',
                                    props: {
                                        message: vm.$t('dialogs.blockDialogSuccess')
                                    }
                                })
                            })
                            .catch(e => {
                                vm.openWindow({
                                    name: 'alert',
                                    props: {
                                        message: vm.$t(`server.errors.${e}`)
                                    }
                                })
                            })
                            .then(() => vm.loading = false);
                    }
                }});
        },
        unBlock(){
            const vm = this;
            vm.loading = true;
            vm.unblockDialog(vm.dialog.id)
                .then(() => {
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: vm.$t('dialogs.unblockDialogSuccess')
                        }
                    })
                })
                .catch(e => {
                    vm.openWindow({
                        name: 'alert',
                        props: {
                            message: vm.$t(`server.errors.${e}`)
                        }
                    })
                })
                .then(() => vm.loading = false);
        },
        addToCRM(){
            const vm = this,
                token = vm.syncToken('crm').token;

            if(token){
                vm.loading = true;

                let client = JSON.parse(JSON.stringify(vm.dialog.client));
                axios({
                    method: 'post',
                    url: `${process.env.SendClientsToCRM}?lang=${vm.$i18n.locale}`,
                    data: client,
                    headers: {
                        Authorization: token
                    }
                })
                    .then(response => {
                        if(response.status === 200){
                            vm.openWindow({
                                name: 'alert',
                                props: {
                                    message: vm.$t('dialogs.addClientToCRMSuccess')
                                }
                            });
                        }
                        else {
                            vm.openWindow({
                                name: 'alert',
                                props: {
                                    message: vm.$t('server.errors.ServiceError')
                                }
                            });
                        }
                    })
                    .catch(e => {
                        if(e.response.status === 400) {
                            vm.openWindow({
                                name: 'alert',
                                props: {
                                    message: vm.$t(`server.errors.${e.response.data.errors}`)
                                }
                            })
                        }
                        else {
                            vm.openWindow({
                                name: 'alert',
                                props: {
                                    message: vm.$t('server.errors.ServiceError')
                                }
                            });
                        }
                    })
                    .then(() => vm.loading = false);
            }
            else {
                vm.openWindow({
                    name: 'alert',
                    props: {
                        message: vm.$t('server.errors.NotSyncWithCRM')
                    }
                })
            }
        },
        openContacts(){
            this.contactsView = !this.contactsView;
        }
    }
}
</script>