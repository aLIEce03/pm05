<template>
  <div class="modal" v-if="isOpen" @click.self="close">
    <div class="modal-content">
      <button class="modal-close" @click="close">✕</button>

      <h2>Оставить заявку</h2>

      <form class="request-form" @submit.prevent="submitForm">
        <label>Ваше имя *</label>
        <input type="text" v-model="form.name" placeholder="Иван Петров" />
        <span class="error" v-if="errors.name">{{ errors.name }}</span>

        <label>Телефон *</label>
        <input type="tel" v-model="form.phone" placeholder="+7 (900) 123-45-67" />
        <span class="error" v-if="errors.phone">{{ errors.phone }}</span>

        <label>Email</label>
        <input type="email" v-model="form.email" placeholder="ivan@mail.ru" />
        <span class="error" v-if="errors.email">{{ errors.email }}</span>

        <label>Компания *</label>
        <input type="text" v-model="form.company" placeholder="ООО Ромашка" />
        <span class="error" v-if="errors.company">{{ errors.company }}</span>

        <label>Дата создания</label>
        <input type="date" v-model="form.date" :min="today" />

        <label>Что нужно создать?</label>
        <textarea v-model="form.message" placeholder="Опишите ваш проект..."></textarea>

        <button type="submit" :disabled="isSubmitting">
          {{ isSubmitting ? 'Отправка...' : 'Подать заявку' }}
        </button>

        <div v-if="statusMessage" :class="statusClass">
          {{ statusMessage }}
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RequestModal',
  props: {
    isOpen: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      isSubmitting: false,
      statusMessage: '',
      statusType: '',
      form: {
        name: '',
        phone: '',
        email: '',
        company: '',
        date: '',
        message: ''
      },
      errors: {
        name: '',
        phone: '',
        email: '',
        company: ''
      }
    }
  },
  computed: {
    today() {
      const now = new Date()
      const year = now.getFullYear()
      const month = String(now.getMonth() + 1).padStart(2, '0')
      const day = String(now.getDate()).padStart(2, '0')
      return `${year}-${month}-${day}`
    },
    statusClass() {
      return this.statusType === 'success' ? 'success-message' : 'error-message'
    }
  },
  watch: {
    isOpen(val) {
      if (val) {
        this.statusMessage = ''
        this.statusType = ''
        this.errors = {
          name: '',
          phone: '',
          email: '',
          company: ''
        }
        if (this.statusType === 'success') {
          this.form.name = ''
          this.form.phone = ''
          this.form.email = ''
          this.form.company = ''
          this.form.message = ''
          this.form.date = this.today
          this.statusMessage = ''
          this.statusType = ''
        }
        if (!this.form.date) {
          this.form.date = this.today
        }
      }
    }
  },
  methods: {
    close() {
      this.$emit('close')
    },
    validateForm() {
      let isValid = true

      if (!this.form.name.trim()) {
        this.errors.name = 'Имя обязательно для заполнения'
        isValid = false
      } else if (this.form.name.trim().length < 2) {
        this.errors.name = 'Имя должно содержать минимум 2 символа'
        isValid = false
      } else {
        this.errors.name = ''
      }

      const phoneClean = this.form.phone.replace(/[\s\-()]/g, '')
      if (!this.form.phone.trim()) {
        this.errors.phone = 'Телефон обязателен для заполнения'
        isValid = false
      } else if (!/^\+?\d{10,15}$/.test(phoneClean)) {
        this.errors.phone = 'Введите корректный номер телефона (10-15 цифр)'
        isValid = false
      } else {
        this.errors.phone = ''
      }

      if (this.form.email && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.form.email)) {
        this.errors.email = 'Введите корректный email'
        isValid = false
      } else {
        this.errors.email = ''
      }

      if (!this.form.company.trim()) {
        this.errors.company = 'Название компании обязательно'
        isValid = false
      } else {
        this.errors.company = ''
      }

      return isValid
    },
    async submitForm() {
      if (!this.validateForm()) {
        const firstError = document.querySelector('.error:not(:empty)')
        if (firstError) {
          firstError.scrollIntoView({ behavior: 'smooth', block: 'center' })
        }
        return
      }

      this.isSubmitting = true
      this.statusMessage = ''
      this.statusType = ''

      const payload = {
        name: this.form.name.trim(),
        phone: this.form.phone.trim(),
        email: this.form.email.trim(),
        company: this.form.company.trim(),
        date: this.form.date,
        message: this.form.message.trim() || 'Не указано'
      }

      try {
        const response = await fetch('/api/request', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(payload)
        })

        const result = await response.json()

        if (response.ok) {
          this.statusType = 'success'
          this.statusMessage = '✅ Заявка успешно отправлена! Мы свяжемся с вами в ближайшее время.'
          this.form.name = ''
          this.form.phone = ''
          this.form.email = ''
          this.form.company = ''
          this.form.message = ''
          this.form.date = this.today
          this.errors = { name: '', phone: '', email: '', company: '' }
        } else {
          this.statusType = 'error'
          this.statusMessage = '❌ ' + (result.message || 'Не удалось отправить заявку. Попробуйте позже.')
        }
      } catch (error) {
        console.error('Ошибка отправки:', error)
        this.statusType = 'error'
        this.statusMessage = '❌ Ошибка соединения с сервером. Проверьте интернет-соединение.'
      } finally {
        this.isSubmitting = false
      }
    }
  }
}
</script>

<style scoped>
/* ===== МОДАЛКА ===== */
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(6px);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    padding: 30px;
}

.modal-content {
    background: #ffffff;
    border-radius: 28px;
    padding: 50px 40px 40px;
    max-width: 640px;
    width: 100%;
    max-height: 92vh;
    min-height: 70vh;
    overflow-y: auto;
    position: relative;
    box-shadow: 0 30px 80px rgba(0, 0, 0, 0.35);
}

/* Стилизуем скролл внутри модалки */
.modal-content::-webkit-scrollbar {
    width: 6px;
}

.modal-content::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 10px;
}

.modal-content::-webkit-scrollbar-thumb {
    background: #0b87cf;
    border-radius: 10px;
}

.modal-close {
    position: absolute;
    top: 18px;
    right: 22px;
    font-size: 28px;
    background: none;
    border: none;
    cursor: pointer;
    color: #888;
    transition: .3s;
    line-height: 1;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.modal-close:hover {
    color: #1d2338;
    background: #f0f4f8;
    transform: rotate(90deg);
}

.modal-content h2 {
    text-align: center;
    font-size: 30px;
    font-weight: 700;
    color: #1e4f78;
    margin-bottom: 36px;
    padding-right: 20px;
}

/* ===== ФОРМА В МОДАЛКЕ ===== */
.request-form label {
    display: block;
    margin-bottom: 10px;
    font-size: 14px;
    font-weight: 600;
    color: #1d2338;
}

.request-form input,
.request-form textarea {
    width: 100%;
    padding: 14px 18px;
    margin-bottom: 20px;
    border: 1.5px solid #d7dce0;
    border-radius: 25px;
    font-size: 16px;
    outline: none;
    font-family: inherit;
    transition: border-color .2s, box-shadow .2s;
    background: #fafcfe;
}

.request-form input:focus,
.request-form textarea:focus {
    border-color: #0b87cf;
    box-shadow: 0 0 0 4px rgba(11, 135, 207, 0.08);
}

.request-form textarea {
    height: 120px;
    resize: vertical;
    min-height: 80px;
}

.error {
    display: block;
    color: #e74c3c;
    font-size: 12px;
    margin-bottom: 10px;
    min-height: 18px;
    padding-left: 4px;
}

.request-form button {
    width: 100%;
    height: 58px;
    border: none;
    border-radius: 25px;
    background: #0b87cf;
    color: #ffffff;
    font-size: 17px;
    font-weight: 600;
    cursor: pointer;
    transition: .25s;
    margin-top: 12px;
    letter-spacing: 0.3px;
}

.request-form button:hover:not(:disabled) {
    background: #1e4f78;
    box-shadow: 0 8px 25px rgba(11, 135, 207, 0.3);
    transform: translateY(-1px);
}

.request-form button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

/* ===== СООБЩЕНИЯ ОБ ОШИБКАХ И УСПЕХЕ ===== */

/* Успешное сообщение — зелёное */
.success-message {
    margin-top: 18px;
    padding: 14px 18px;
    border-radius: 14px;
    text-align: center;
    font-weight: 500;
    font-size: 15px;
    background: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
}

/* Сообщение об ошибке — красное */
.error-message {
    margin-top: 18px;
    padding: 14px 18px;
    border-radius: 14px;
    text-align: center;
    font-weight: 500;
    font-size: 15px;
    background: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
}

/* ===== АДАПТИВ ДЛЯ МОДАЛКИ ===== */
@media (max-width: 768px) {
    .modal {
        padding: 16px;
    }

    .modal-content {
        padding: 32px 24px 28px;
        max-height: 90vh;
        min-height: auto;
        border-radius: 20px;
    }

    .modal-content h2 {
        font-size: 26px;
        margin-bottom: 28px;
    }

    .request-form input,
    .request-form textarea {
        padding: 12px 16px;
        font-size: 15px;
        margin-bottom: 16px;
    }

    .request-form button {
        height: 52px;
        font-size: 15px;
    }
}

@media (max-width: 480px) {
    .modal {
        padding: 10px;
        align-items: flex-end;
    }
    
    .modal-content {
        padding: 24px 16px 20px;
        min-height: auto;
        max-height: 95vh;
        border-radius: 16px 16px 0 0;
        margin-bottom: 0;
    }

    .modal-content h2 {
        font-size: 22px;
        margin-bottom: 20px;
        padding-right: 10px;
    }

    .request-form label {
        font-size: 13px;
        margin-bottom: 6px;
    }
    
    .request-form input,
    .request-form textarea {
        padding: 10px 14px;
        font-size: 14px;
        border-radius: 20px;
        margin-bottom: 12px;
    }

    .request-form textarea {
        height: 80px;
        min-height: 60px;
    }

    .request-form button {
        height: 46px;
        font-size: 14px;
        border-radius: 20px;
        margin-top: 8px;
    }

    .modal-close {
        top: 12px;
        right: 12px;
        font-size: 20px;
        width: 32px;
        height: 32px;
    }

    .error {
        font-size: 11px;
        min-height: 16px;
        margin-bottom: 6px;
    }

    .success-message,
    .error-message {
        font-size: 13px;
        padding: 10px 14px;
        margin-top: 12px;
    }
}
</style>