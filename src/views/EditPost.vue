<template>
    <v-container>
        <v-row no-gutters>
            <v-col sm="10" class="mx-auto">
                <v-card class="pa-5">
                    <v-card-title>Edit This Post</v-card-title>
                    <v-divider></v-divider>
                    <v-form ref="form" @submit.prevent="updateForm" class="pa-5" enctype="multipart/form-data">
                        <v-text>BAGAIMANA KABARMU HARI INI?</v-text>
                        <v-textarea label="Jawab" v-model="post.content" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text>APA 3 HAL YANG HARUS KAMU KERJAKAN HARI INI?</v-text>
                        <v-textarea label="Jawab" v-model="post.content2" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text>BAGAIMANA PERASAANMU?</v-text>
                        <v-textarea label="Jawab" v-model="post.content3" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text>KEGIATAN APA YANG HARI INI KAMU LAKUKAN?</v-text>
                        <v-textarea label="Jawab" v-model="post.content4" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text>APA YANG KAMU AMATI HARI INI?</v-text>
                        <v-textarea label="Jawab" v-model="post.content5" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text>APA YANG BISA KAMU SYUKURI HARI INI?</v-text>
                        <v-textarea label="Jawab" v-model="post.content6" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        
                        <v-file-input @change="selectFile" show-size counter multiple label="Select Image"></v-file-input>
                        <v-img :src="`/${post.image}`" width="120"></v-img>

                        <v-btn type="submit" class="mt-3" color="success">Update Post</v-btn>
                    </v-form>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import API from '../api';
export default {
    data() {
        return {
            rules: [(value) => !!value || "This field is required!"],
            post: {
                title: "BAGAIMANA KABARMU HARI INI?",
                content: "",
                title2: "APA 3 HAL YANG HARUS KAMU KERJAKAN HARI INI?",
                content2: "",
                title3: "BAGAIMANA PERASAANMU?",
                content3: "",
                title4: "KEGIATAN APA YANG HARI INI KAMU LAKUKAN?",
                content4: "",
                title5: "APA YANG KAMU AMATI HARI INI?",
                content5: "",
                title6: "APA YANG BISA KAMU SYUKURI HARI INI?",
                content6: "",
                category: "",
                image: ""
            },
            image: "",
        };
    },
    async created() {
        const response = await API.getPostByID(this.$route.params.id);
        this.post = response
    },
    methods: {
        selectFile(file) {
            this.image = file[0];
        },
        async updateForm() {
            const formData = new FormData();
            formData.append("image", this.image);
            formData.append("title", this.post.title);
            formData.append("title2", this.post.title2);
            formData.append("title3", this.post.title3);
            formData.append("title4", this.post.title4);
            formData.append("title5", this.post.title5);
            formData.append("title6", this.post.title6);
            formData.append("category", this.post.category);
            formData.append("content", this.post.content);
            formData.append("content2", this.post.content2);
            formData.append("content3", this.post.content3);
            formData.append("content4", this.post.content4);
            formData.append("content5", this.post.content5);
            formData.append("content6", this.post.content6);
            formData.append("old_image", this.post.image);
            if(this.$refs.form.validate()){
                const response = await API.updatePost(this.$route.params.id, formData);
                console.log(response);
                this.$router.push({ name: "home", params: {message: response.message}});
            }
        },
    },
};
</script>