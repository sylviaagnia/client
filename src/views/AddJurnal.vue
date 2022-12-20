<template>
    <v-container>
        <v-row no-gutters>
            <v-col sm="10" class="mx-auto">
                <v-card class="pa-5">
                    <v-card-title>Add New Jurnal</v-card-title>
                    <v-divider></v-divider>
                    <v-form ref="form" @submit.prevent="submitForm" class="pa-5" enctype="multipart/form-data">
                        <v-text label="Pertanyaan" v-model="post.title" prepend-icon="mdi-note" :rules="rules">BAGAIMANA KABARMU HARI INI</v-text>
                        <v-textarea label="Jawab" v-model="post.content" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text label="Pertanyaan2" v-model="post.title2" prepend-icon="mdi-note" :rules="rules">APA 3 HAL YANG HARUS KAMU KERJAKAN HARI INI?</v-text>
                        <v-textarea label="Jawab" v-model="post.content2" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text label="Pertanyaan3" v-model="post.title3" prepend-icon="mdi-note" :rules="rules">BAGAIMANA PERASAANMU?</v-text>
                        <v-textarea label="Jawab" v-model="post.content3" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text label="Pertanyaan4" v-model="post.title4" prepend-icon="mdi-note" :rules="rules">KEGIATAN APA YANG HARI INI KAMU LAKUKAN?</v-text>
                        <v-textarea label="Jawab" v-model="post.content4" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text label="Pertanyaan5" v-model="post.title5" prepend-icon="mdi-note" :rules="rules">APA YANG KAMU AMATI HARI INI?</v-text>
                        <v-textarea label="Jawab" v-model="post.content5" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text label="Pertanyaan6" v-model="post.title6" prepend-icon="mdi-note" :rules="rules">APA YANG BISA KAMU SYUKURI HARI INI?</v-text>
                        <v-textarea label="Jawab" v-model="post.content6" prepend-icon="mdi-note-plus" :rules="rules"></v-textarea>
                        <v-text-field label="Category" v-model="post.category" prepend-icon="mdi-view-list" :rules="rules"></v-text-field>
                        <v-file-input @change="selectFile" :rules="rules" show-size counter multiple label="Select Image"></v-file-input>
                        <v-btn type="submit" class="mt-3" color="primary">Add Jurnal</v-btn>
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
    methods: {
        selectFile(file) {
            this.image = file[0];
        },
        async submitForm() {
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
            if(this.$refs.form.validate()){
                const response = await API.addJurnal(formData);
                console.log(response);
                this.$router.push({ name: "home", params: {message: response.message}});
            }
        },
    },
};
</script>