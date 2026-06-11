<script setup>
import { useRouter } from 'vue-router';
import MyInput from '../../components/input/MyInput.vue';
import { reactive, ref } from 'vue';
import MyButton from '../../components/button/MyButton.vue';
import { useFileStore } from '../../store/file/useFileStore';
import uploadVaildator from '../../util/validator/domain/auth/uploadVaildator.js';
import { useMyErrorStore } from '../../store/error/useMyErrorStore.js';

const router = useRouter();
const fileStore = useFileStore()
const myErrorStore = useMyErrorStore();

const preview = ref(null);
const selectedFile = ref(null);
const content = ref('');

const uploadData = reactive({
  text: '',
  img: '',
});

const handleUpload = async (e) => {
  const file = e.target.files[0];

    if(file) {
    if(preview.value) {
      URL.revokeObjectURL(preview.value);
    }
    preview.value = URL.createObjectURL(file);

    const fileUri = await fileStore.uploadPost(file);
    
    if(fileUri) {
      uploadData.img = fileUri;

      selectedFile.value = file;
    }
  }
}

const handleSubmit = async () => {

  const validationList = [
    uploadVaildator.text(uploadData.text),
    uploadVaildator.img(uploadData.img),
  ]; 
  const errorList = validationList.filter(val => val);

  if(errorList.length > 0) {
    alert(errorList.join('\n'));
    return;
  }

  try {
    const response = await fileStore.createPost(uploadData);
    alert('작성완료');

    router.replace('/');
  } catch (error) {
  myErrorStore.setErrorInfo(error);
  router.replace('/errors');
  }  
}
  </script>

  <template>
  <form @submit.prevent="handleSubmit">

    <textarea
      class="text-box"
      v-model="uploadData.text"
      :placeholder="'내용 작성'"
    ></textarea>

    <div 
      class="preview"
      v-if="preview"
      :style="{backgroundImage: `url(${preview})`}"
    ></div>

  <input
    type="file"
    accept="image/*"
    @change="handleUpload"
  >

  <MyButton
    class="btn"
    :btnType="'submit'"
    :content="'Write'"
    :color="'gray'"
    :size="'middle'"
  ></MyButton>

  </form>
  </template>

  <style scoped>
  form {
    padding: 20px 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
  }

  .preview {
    width: 200px;
    height: 200px;
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
  }

  .text-box {
    width: 80%;
    height: 200px;

    font-size: 20px;
    padding: 20px;
    box-sizing: border-box;
  }

  .btn {
    margin-top: 100px;
  }

  </style>
