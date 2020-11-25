<template>
  <div class="modal__bg" @click="testClick">
    <!-- <p>
      {{ this.modalData }}
    </p> -->
    <div :class="this.currentSlideClass">
      <div class="modal__bg-border">
        <img v-if="modalData.background_image.src" loading="lazy" :src="modalData.background_image.src" :alt="modalData.background_image.alt">
        <img :src="background" loading="lazy" aria-hidden alt="Arbor">
      </div>
      <div :class='this.modalData.use_form ? "modal__slide modal__slide--1" : "modal__slide modal__slide--1 modal__slide--single"'>
        <div class="modal__slide__wrapper">
          <div class="modal__slide__text" v-html="this.modalData.slide_1_content"></div>
          <button v-if="this.modalData.use_form" @click="nextSlide">{{this.modalData.next_button_text}}</button>
        </div>
      </div>
      <div v-if="this.modalData.use_form" class="modal__slide modal__slide--2">
        <div class="modal__slide__wrapper">
          <div id="modal__form"></div>
        </div>
        </div>
      <div v-if="this.modalData.use_form" class="modal__slide modal__slide--3">
        <div class="modal__slide__wrapper">
            <div class="modal__slide__text" v-html="this.modalData.slide_3_content"></div>
        </div>
        </div>
      <div class="modal__close" @click="triggerClose">
          <img :src="ex" loading="lazy" alt="Arbor">
      </div>
    </div>
  </div>
</template>

<script>

import background from '@/assets/arbor-background-pattern.svg';
import ex from '@/assets/x.svg';

export default {
  name: 'Modal',
  props: ['modalData'],
  data: function() {
    return {
      placeholder: true,
      currentSlide: 0,
      background,
      ex
    };
  },
  created: function() {
    if (this.modalData.form.form_id) {
      hbspt.forms.create({ 
          portalId: '747395',
          target: '#modal__form',
          // add formId
          formId: this.modalData.form.form_id,
          onFormReady: function($form) {
            setTimeout(() => {
              var selectCheck = $('.modal__body .selectric');
              if (selectCheck) {
                // console.log("selectric found");
                $('.modal__body select').selectric("destroy");
              }
            }, 500);
            // console.log($form[0].dataset);
            var formID = $form[0].dataset.formId;
            // console.log('document.querySelector(form[data-form-id="\'' + formID + '\'"]\')');
            var generatedForm = document.querySelector('form[data-form-id="' + formID + '"]');
            // console.log(generatedForm);
            var selects = generatedForm.querySelectorAll('select');
            // console.log(selects);
            selects.forEach( select => {
              select.addEventListener("change", e => {
                // console.log(e);
                swapLabels(formID);
              })
            })
            // window.addEventListener('message', event => {
            //   if(event.data.type === 'hsFormCallback' && event.data.eventName === 'onFormReady') {
            //       //console.log(event.data.id);
            //       swapLabels(event.data.id);
            //   }
            // });
          },
          onFormSubmit: function($form) {
            $vm.$refs.modal.nextSlide();
          } 
      });
    }
  },
  mounted: function() {
    addVueVar();
    // $(".modal__body select").selectric('destroy');
    function swapLabels(formID) {
      var modalFormFields = document.querySelectorAll('#modal__form form[data-form-id="' + formID + '"] .hs-form-field');
      modalFormFields.forEach(field => {
        if (field.classList.contains("hs-fieldtype-text")) {
          var label = field.querySelector("label span");
          var input = field.querySelector("input");
          input.placeholder = label.innerHTML;
        } else if (field.classList.contains("hs-fieldtype-select")) {
          var label = field.querySelector("label span");
          var input = field.querySelector("select option:first-child");
          input.innerHTML= label.innerHTML;
        }
      });
    };
    if (this.modalData.slide_1_content.indexOf('data-cta') !== -1) {
      const ctas = document.querySelectorAll('*[data-cta]');
      ctas.forEach(cta => {
        const ctaID = cta.dataset.cta;
        // console.log(`CTA ID IS ${ctaID}`);
        console.log(cta.classList);
        cta.classList.add("hs-cta-wrapper");
        cta.setAttribute("id", `hs-cta-wrapper-${ctaID}`);
        
        const nodeSpan = document.createElement('span');
        nodeSpan.classList.add("hs-cta-node", `hs-cta-${ctaID}`);
        nodeSpan.setAttribute("id", `hs-cta-${ctaID}`);

        const linkWrap = document.createElement('a');
        linkWrap.setAttribute("href", `https://cta-redirect.hubspot.com/cta/redirect/${ctaID}`);
        linkWrap.setAttribute("target", "_blank");

        const imgPlaceholder = document.createElement('img');
        imgPlaceholder.classList.add("hs-cta-img");
        imgPlaceholder.setAttribute("id", `hs-cta-img-${ctaID}`);
        imgPlaceholder.setAttribute("src", `https://no-cache.hubspot.com/cta/default/747395/${ctaID}.png`);

        linkWrap.appendChild(imgPlaceholder);
        nodeSpan.appendChild(linkWrap);
        cta.appendChild(nodeSpan);

        setTimeout(() => {
          hbspt.cta.load(747395, `${ctaID}`, {});
        }, 100);

      })
    };
    if (this.modalData.slide_3_content.indexOf('data-cta') !== -1) {
      const ctas = document.querySelectorAll('*[data-cta]');
      ctas.forEach(cta => {
        const ctaID = cta.dataset.cta;
        // console.log(`CTA ID IS ${ctaID}`);
        console.log(cta.classList);
        cta.classList.add("hs-cta-wrapper");
        cta.setAttribute("id", `hs-cta-wrapper-${ctaID}`);
        
        const nodeSpan = document.createElement('span');
        nodeSpan.classList.add("hs-cta-node", `hs-cta-${ctaID}`);
        nodeSpan.setAttribute("id", `hs-cta-${ctaID}`);

        const linkWrap = document.createElement('a');
        linkWrap.setAttribute("href", `https://cta-redirect.hubspot.com/cta/redirect/${ctaID}`);
        linkWrap.setAttribute("target", "_blank");

        const imgPlaceholder = document.createElement('img');
        imgPlaceholder.classList.add("hs-cta-img");
        imgPlaceholder.setAttribute("id", `hs-cta-img-${ctaID}`);
        imgPlaceholder.setAttribute("src", `https://no-cache.hubspot.com/cta/default/747395/${ctaID}.png`);

        linkWrap.appendChild(imgPlaceholder);
        nodeSpan.appendChild(linkWrap);
        cta.appendChild(nodeSpan);

        setTimeout(() => {
          hbspt.cta.load(747395, `${ctaID}`, {});
        }, 100);

      })
    }
  },
  computed: {
    // currentSlideClass() {
    //   let bgImage = this.props.moduleData.background_image.src.length > 0;
    //   return "modal__body modal__body--slide-" + this.currentSlide + "modal__body--bg-" + bgImage;
    // },
    currentSlideClass() {
      return "modal__body modal__body--slide-" + this.currentSlide + " modal__body--style--" + this.modalData.pop_up_style;
    },
  },
  methods: {
    nextSlide() {
      this.currentSlide += 1;
      // $(".modal__body select").selectric('destroy');
      var selectCheck = $('.modal__body .selectric');
      if (selectCheck) {
        console.log("selectric found");
        $('.modal__body select').selectric("destroy");
      }
    },
    triggerClose() {
      this.$emit("close", true);
    },
    testClick(e) {
      if (e.target.className == "modal__bg") {
        this.triggerClose();
      }
    }
  },
};
</script>

<style lang="scss">
.modal {
  &__bg {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    background: rgba(0,0,0,0.5);
    height: 100vh;
    width: 100vw;
    display: grid;
    justify-content: center;
    align-content: center;
    z-index: 999999999999999;
    &-border {
      position: absolute;
      height: 100%;
      img {
        height: 100%;
        position: relative;
        z-index: 1;
        max-width: 161px;
        object-fit: cover;
        & + img {
          z-index: 0;
          transform: translateX(-36px);
        }
      }
    }
  }
  &__body {
    max-width: 600px;
    display: grid;
    grid-template-columns: 100% 100% 100%;
    overflow: hidden;
    position: relative;
    h1, h2, h3, h4, h5 {
      letter-spacing: -0.77px;
      line-height: 1.2;
      font-weight: 400;
    }
    h2 {
      @media screen and (min-width: 1100px) {
        font-size: 32px;
      }
    }
    p, ul, ol, li, blockquote {
      color: #fff;
    }
    p {
      line-height: 1.6;
      font-size: 14px;
      @media screen and (min-width: 768px) {
        font-size: 16px;
      }
    }
    button, input[type="submit"] {
      color: #FFFFFF;
      letter-spacing: -0.43px;
      text-align: center;
      padding: 0.5em 1.5em 0.56em;
      border: none;
      border-radius: 2em;
      cursor: pointer;
      transition: all 0.3s ease;
      background: #00537F;
      &:hover {
        opacity: 0.6;
      }
      @media screen and (min-width: 1100px) {
        font-size: 18px;
      }
    }
    &--slide {
      &-0 {
        .modal__slide {
          transform: translateX(0);
        }
      }
      &-1 {
        .modal__slide {
          transform: translateX(-100%);
        }
      }
      &-2 {
        .modal__slide {
          transform: translateX(-200%);
        }
      }
    }
    &--style {
      &--border {
        max-width: 500px;
        .modal__bg-border img {
          display: none;
          & + img {
            display: block;
            transform: none;
          }
        }
        .modal__slide__wrapper {
          margin-right: auto;
          transform: translateX(1.6rem);
        }
      }
    }
  }
  &__slide {
    padding: 27px 2.8rem 39px;
    transition: all 0.3s ease;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    &__wrapper {
      max-width: 344px;
      margin-left: auto;
    }
    &--single {
      width: 500px;
    }
  }
  &__close {
    position: absolute;
    right: 1rem;
    top: 0.9rem;
    cursor: pointer;
    transition: all 0.3s ease;
    &:hover {
      opacity: 0.6;
    }
  }
  .hs-input::-webkit-input-placeholder {
    color: #4F4E50;
  }
}
#modal__form form {
  margin-bottom: 0;
}
#modal__form form * {
    float: none;
    display: block;
    text-align: initial;
    max-width: 100%;
    margin: 0;
    width: auto;
}

#modal__form form input, #modal__form form textarea {
    width: calc(100% - 34px) !important;
    padding: 11px 16px !important;
}

#modal__form form .input {
    width: 100%;
}

#modal__form form select {
    width: 100%;
}

#modal__form form .actions {
    padding: 0;
}

#modal__form form input[type="submit"] {
    text-align: center;
    width: auto !important;
}

#modal__form form .form-columns-2 > * {width: calc(50% - 15px);display: inline-block;}

#modal__form form .form-columns-2 > *:nth-child(2) {
    margin-left: 28px;
}
#modal__form form input, #modal__form form select {
    border: 2px solid #E5CF9B;
    border-radius: 0;
}

#modal__form form input[type="submit"] {
    border-radius: 2rem;
    background: #00537F;
    margin-top: 1.4rem;
    box-shadow: none;
}
#modal__form form select {
    appearance: none;
    padding: 0 2ch;
    background: #fff;
}

#modal__form form .hs-fieldtype-select .input {
    position: relative;
}
#modal__form form .hs-fieldtype-select .input:after {
    content: "";
    position: absolute;
    width: 0;
    height: 0;
    top: 50%;
    right: 1ch;
    transform: translateY(-64%) scaleX(0.55);
    border-left: 1ch solid transparent;
    border-right: 1ch solid transparent;
    border-top: 1ch solid #01537e;
}
#modal__form .selectric-wrapper.selectric-hs-input {
    height: 39px !important;
}
#modal__form form label.hs-error-msg {
    display: block;
    margin: 0.8em 0 0;
    color: #ec0d0d;
}
@media screen and (max-width: 1100px) {
  .modal__body button, .modal__body input[type="submit"] {
    font-size: 1.6rem;
    margin-top: 0.6em;
}
#modal__form form .hs-fieldtype-booleancheckbox ul label.hs-form-booleancheckbox-display>input {
  padding: 2px !important;
  background: #fff;
}
}
</style>
