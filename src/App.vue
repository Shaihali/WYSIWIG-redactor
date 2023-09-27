<template>
  <div class="wrapper">
    <Navbar>
      <li class="navbar_item">
        <button 
          class="navbar_item-btn btn_undo"
          @click="undo"
          >
        </button>
      </li>
      <li class="navbar_item">
        <button 
          class="navbar_item-btn btn_redo"
          @click="redo"
          >
        </button>
      </li>
      <li class="navbar_item">
        <button 
          class="navbar_item-btn btn_heading"
          @click="convertToHeading"
          >

        </button>
      </li>
      <li class="navbar_item">
        <button 
          class="navbar_item-btn btn_paragraph"
          @click="makeParagraph"
          >
        </button>
      </li>
      <li class="navbar_item">
        <button 
          class="navbar_item-btn btn_img"
          @click="insertImage"
          >
        </button>
      </li>
      <li class="navbar_item text" @click="copyHTML"> Скопировать HTML</li>
    </Navbar>
    <main>
      <div class="content" contenteditable="true" ref="visualView" @input="onInputChange" @mouseup="onInputChange">
        Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций, занимаемых участниками в отношении поставленных задач. С другой стороны постоянное информационно-пропагандистское обеспечение нашей деятельности представляет собой интересный эксперимент проверки форм развития. Идейные соображения высшего порядка, а также укрепление и развитие структуры влечет за собой процесс внедрения и модернизации соответствующий условий активизации. Задача организации, в особенности же реализация намеченных плановых заданий играет важную роль в формировании дальнейших направлений развития. Повседневная практика показывает, что постоянное информационно-пропагандистское обеспечение нашей деятельности играет важную роль в формировании существенных финансовых и административных условий.
      </div>
    </main>
  </div>
</template>

<script>
  import Navbar from './components/Navbar/Navbar.vue';

  function transformTextToTitle() {
    const selection = window.getSelection();
    if (!selection.isCollapsed) {
      const range = selection.getRangeAt(0);
      const text =  `<h1>${range.toString()}</h1>`;
      const fragment = document.createRange().createContextualFragment(text);
      range.deleteContents();
      range.insertNode(fragment);
    } else {
      alert("Пожалуйста, сначала выделите текст");
    }
  };
  function transformTextToParagraph() {
    const selection = window.getSelection();
    if (!selection.isCollapsed) {
      const range = selection.getRangeAt(0);
      const text =  `<p>${range.toString()}</p>`;
      const fragment = document.createRange().createContextualFragment(text);
      range.deleteContents();
      range.insertNode(fragment);
    } else {
      alert("Пожалуйста, сначала выделите текст");
    }
  };
  function copyHtmlToBufffer() {
    const html = document.querySelector('.content').innerHTML;
    navigator.clipboard.writeText(html)
      .then(() => {
        alert('HTML скопирован в буфер обмена');
      })
      .catch((error) => {
        alert('Ошибка при копировании HTML:', error);
      });
  };

  export default {
    components: {
      Navbar
    },
    data() {
      return {
        history: [],
        currentIndex: -1
      };
    },
    mounted() {
      this.onInputChange();
    },
    methods: {
      insertImage() {
        const url = prompt('Введите URL изображения:');
        if (url) {
          const img = document.createElement('img');
          img.setAttribute('style', 'max-width: 800px; width: 100%; height: auto;');
          img.src = url;
          const selection = document.getSelection();
          if (selection.rangeCount > 0) {
            const range = selection.getRangeAt(0);
            range.deleteContents();
            range.insertNode(img);
          }
        }
      },
      convertToHeading() {
        transformTextToTitle();
      },
      makeParagraph() {
        transformTextToParagraph();
      },
      copyHTML() {
        copyHtmlToBufffer();
      },
      onInputChange() {
        const content = this.$refs.visualView.innerHTML;
        this.history = this.history.slice(0, this.currentIndex + 1);
        this.history.push(content);
        
        this.currentIndex++;
    },
      undo() {
        if (this.currentIndex > 0) {
          this.currentIndex--;
          this.$refs.visualView.innerHTML = this.history[this.currentIndex];
        }
      },
      redo() {
        if (this.currentIndex < this.history.length - 1) {
          this.currentIndex++;
          this.$refs.visualView.innerHTML = this.history[this.currentIndex];
        }
      }
    },
  }
</script>

<style>
  body {
    background-color: #000;
    margin: 0;
  }
  .wrapper {
    max-width: 1100px;
    margin: 0 auto;
    padding: 10px;
  }
  .content {
    color: #fff;
  }
  .navbar_item-btn {
    background-repeat: no-repeat;
    background-position: center;
    width: 42px;
    height: 38px;
    cursor: pointer;
  }
  .btn_undo {
    background-image: url('./assets/icons/left.png');
  }
  .btn_redo {
    background-image: url('./assets/icons/right.png');
  }
  .btn_heading {
    background-image: url('./assets/icons/T.png');
  }
  .btn_paragraph {
    background-image: url('./assets/icons/tt.png');
  }
  .btn_img {
    background-image: url('./assets/icons/img.png');
  }
  .navbar_item {
    list-style-type: none;
    cursor: pointer;
  }
  .text {
    color: blueviolet;
    font-size: 18px;
  }
</style>
