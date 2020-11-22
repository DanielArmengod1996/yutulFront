<template>
    <div class="card mx-xl-5">
        <!-- Card body -->
        <div class="card-body">
        <!-- Default form subscription -->
        <b-form class="m-5" @submit="submitContent">
            <h2>Upload Video</h2>
            <!--para la subida del video-->
            <b-container class="bv-example-row">
                <b-row>
                    <b-col>
                    <!--para la subida del video-->
                    <b-form-file
                        required
                        v-if="!isUploadedVideo && !isUploadingVideo"
                        accept="video/*"
                        v-model="fileVideo"
                        :state="Boolean(fileVideo)"
                        placeholder="Drop video here..."
                        drop-placeholder="Drop file here..."
                    ></b-form-file>
                    <b-button variant="primary" disabled v-if="isUploadingVideo">
                        <b-spinner small type="grow"></b-spinner>Uploading video...
                    </b-button>
                    <div
                        class="alert alert-success form-custom m-1"
                        v-if="isUploadedVideo"
                        role="alert"
                    >Video uploaded sucessfully!!</div>
                    </b-col>
                    <b-col>
                    <!--para la subida de la portada-->
                    <b-form-file
                        id="inputImageFile"
                        required
                        v-if="!isUploadedImage && !isUploadingImage"
                        accept="image/*"
                        v-model="fileImage"
                        :state="Boolean(fileImage)"
                        placeholder="Drop image here..."
                        drop-placeholder="Drop file here..."
                    ></b-form-file>
                    <b-button variant="primary" disabled v-if="isUploadingImage">
                        <b-spinner small type="grow"></b-spinner>Uploading image...
                    </b-button>
                    <div
                        class="alert alert-success form-custom m-1"
                        v-if="isUploadedImage"
                        role="alert"
                    >Image uploaded sucessfully!!</div>
                    <!-- logging que se mostrará cuando el usuario quiera guardar los cambios y subir video-->
                    </b-col>
                </b-row>
                <b-row>
                    <b-col>
                    <!-- title -->
                    <input
                        type="text"
                        v-model="form.title"
                        required
                        placeholder="title"
                        id="title"
                        class="form-control form-custom m-1"
                    />
                    </b-col>
                    <b-col>
                    <!-- tags -->
                    <input
                        type="text"
                        v-model="form.tags"
                        required
                        placeholder="tags : separated with comma ('tag1,tag2,...')"
                        id="tags"
                        class="form-control form-custom m-1"
                    />
                    </b-col>
                </b-row>
                <b-row rows="5">
                    <b-col>
                    <b-form-textarea
                        v-model="form.description"
                        type="text-area"
                        placeholder="description"
                        id="description"
                        class="form-control m-1"
                        height="15rem"
                    />
                    </b-col>
                    <b-col></b-col>
                </b-row>
                <div class="text-center py-4 mt-3">
                    <button
                    v-if="!isUploadedImage || !isUploadedVideo"
                    disabled
                    type="submit"
                    class="btn btn-primary btn-lg btn-block"
                    >
                    Send
                    <i class="far fa-paper-plane ml-2"></i>
                    </button>
                    <button v-else type="submit" class="btn btn-primary btn-lg btn-block">
                    Send
                    <i class="far fa-paper-plane ml-2"></i>
                    </button>
                </div>
            </b-container>

            <!-- Default input title -->
        </b-form>
        <!-- Default form subscription -->
        </div>
        <!-- image cropper thats opens when the user upload a image -->
        <div id="app">
        <div v-if="showModal">
            <transition name="modal">
            <div class="modal-mask">
                <div class="modal-wrapper">
                    <div class="modal-dialog" role="document">
                        <div class="modal-content w-100">
                        <CropImage
                            :srcImage="this.uploadImageUrl"
                            v-on:uploadImage="uploadImage"
                            v-on:closeModal="closeModal"
                        />
                        </div>
                    </div>
                </div>
            </div>
            </transition>
        </div>
        </div>
    </div>
</template>

<script>
// controller
import axios from "axios";

import CropImage from "./CropImage.vue";
export default {
    components: {
        CropImage
    },
    props: {
        userId: String
    },
    data() {
        return {
            idMedia: null,
            fileVideo: null,
            fileImage: null,
            isUploadingVideo: false,
            isUploadedVideo: false,
            isUploadedImage: false,
            isUploadingImage: false,
            form: {
                id_usuario:( this.userId == 'null' ) ? null : this.userId,
                url_video: null,
                url_image: null,
                title: null,
                tags: null,
                description: null,
            },
            showModal: false,
            uploadImageUrl: null
        };
    },
    methods: {
        closeModal() {
            this.showModal = false;
        },
        uploadVideo() {
            this.isUploadingVideo = true;
            let formData = new FormData();
            var fileName = this.idMedia + ".mp4";
            formData.append("file", this.fileVideo, fileName);
            this.form.url_video = fileName;

        // You should have a server side REST API
            axios.post("http://localhost:8020/uploadMedia", formData, {
                "Content-type": "multipart/form-data",
                "Access-Control-Allow-Origin": "*"
            })
            .then(() => {
                this.isUploadingVideo = false;
                this.isUploadedVideo = true;
            });
        },
        uploadImage(image) {
            //console.log(image);
            const data = image.src;
            const block = data.split(";");
            var contentType = block[0].split(":")[1];
            var realData = block[1].split(",")[1];
            var blob = this.b64toBlob(realData, contentType);

            this.showModal = false;
            this.isUploadingImage = true;

            let formData = new FormData();
            var fileName = this.idMedia + ".png";
            formData.append("file", blob, fileName);
            this.form.url_image = fileName;

            // You should have a server side REST API
            axios.post("http://localhost:8020/uploadMedia", formData, {
            "Content-type": "multipart/form-data",
            "Access-Control-Allow-Origin": "*"
            })
            .then(() => {
            this.isUploadingImage = false;
            this.isUploadedImage = true;
            });
        },
        b64toBlob(b64Data, contentType, sliceSize) {
            contentType = contentType || "";
            sliceSize = sliceSize || 512;

            var byteCharacters = atob(b64Data);
            var byteArrays = [];

            for (var offset = 0; offset < byteCharacters.length; offset += sliceSize ) {
                var slice = byteCharacters.slice(offset, offset + sliceSize);

                var byteNumbers = new Array(slice.length);
                for (var i = 0; i < slice.length; i++) {
                byteNumbers[i] = slice.charCodeAt(i);
                }

                var byteArray = new Uint8Array(byteNumbers);

                byteArrays.push(byteArray);
            }

            var blob = new Blob(byteArrays, { type: contentType });
            return blob;
        },
        handleFileUpload() {

        this.fileVideo = this.$refs.fileVideo
            ? this.$refs.fileVideo.files[0]
            : null;
        this.fileImage = this.$refs.fileImage
            ? this.$refs.fileImage.files[0]
            : null;
        },
        submitContent() {
            axios.post("http://localhost:8020/setVideo", JSON.stringify(this.form), {
                "Content-type": "multipart/form-data",
                "Access-Control-Allow-Origin": "*"
            })
            .then(resp => {
                alert(resp.data);
            });
        }
    },
    watch: {
        fileVideo: function(file) {
            alert(this.form.id_usuario);
            if (file) {
                this.uploadVideo();
            }
        },
        fileImage: function(file) {
            alert(this.form.id_usuario);
            if (file) {
                this.uploadImageUrl = URL.createObjectURL(file);
                this.showModal = true;
            }
        }
    },
    created() {
        var current_date = btoa(
            new Date().valueOf().toString() + (Math.random() * 99999).toString()
        );

        this.idMedia = this.form.id_usuario + current_date;
    }
};
</script>