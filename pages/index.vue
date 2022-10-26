<template>
  <div class="container">
    <CBox
      left = "10px"
      top = "10px"
      v-bind="mainStyles[colorMode]"
      position="absolute"
      v-if="isShowBackwardBtn"
    >
      <CIconButton
        :icon="'chevron-left'"
        @click="toPreviousPage"
      />
    </CBox>
    <CBox
      position="absolute"
      right="10px"
      top="10px"
      text-align="center">
      <CBox>
        <CIconButton
          :icon="colorMode === 'light' ? 'moon' : 'sun'"
          :aria-label="`Switch to ${
                colorMode === 'light' ? 'dark' : 'light'
              } mode`"
          @click="toggleColorMode"
        />
      </CBox>
    </CBox>
    <CBox
      v-bind="mainStyles[colorMode]"
      d="flex"
      w="100vw"
      h="100vh"
      flex-dir="column"
      pt="10px"
      v-if="page==='auth'"
      v-bind:justify-content="mainBoxJustifyContent"
    >
      <CHeading text-align="center" mb="4" v-if="!isShowRegister">
        Авторизация
      </CHeading>
      <CHeading text-align="center" mb="4" v-if="isShowRegister">
        Регистрация
      </CHeading>
      <CBox mt="4">
        <CBox mb="2">
          <CFlex justify="center" direction="column" align="center">
            <CInput placeholder="Логин" width="24rem" type="login" h="54"/>
          </CFlex>
        </CBox>
        <CBox mb="2">
          <CFlex justify="center" direction="column" align="center">
            <CInput placeholder="Пароль" width="24rem"  h="54" type="password"/>
          </CFlex>
        </CBox>
        <div v-if="isShowRegister">
          <CBox mb="5">
            <CFlex justify="center" direction="column" align="center">
              <CInput placeholder="Повтоорите пароль" width="24rem"  h="54" type="password"/>
            </CFlex>
          </CBox>
          <CBox mb="2">
            <CFlex justify="center" direction="column" align="center">
              <CInput placeholder="Почта" width="24rem"  h="54" type="email"/>
            </CFlex>
          </CBox>
        </div>
      </CBox>
      <CFlex justify="center" direction="column" align="center" mt="3">
        <CBox mb="3" text-align="center">
          <CButton v-if="isShowRegister"
                   right-icon="arrow-up"
                   variant-color="gray"
                   @click="showRegister"
                   mb="4"
          >
            Войти
          </CButton>
          <CButton v-if="!isShowRegister"
                   left-icon="arrow-down"
                   variant-color="gray"
                   @click="showRegister"
                   mb="4"
          >
            Регистрация
          </CButton>
          <CButton v-if="!isShowRegister"
                   right-icon="arrow-forward"
                   variant-color="blue"
                   @click="auth"
                   mb="4"
          >
            Войти
          </CButton>
          <CButton  v-if="isShowRegister"
                    left-icon="arrow-forward"
                    variant-color="blue"
                    @click="auth"
                    mb="4"
          >
            Регистрация
          </CButton>
        </CBox>
      </CFlex>
    </CBox>

    <CBox
      v-bind="mainStyles[colorMode]"
      d="flex"
      w="100vw"
      h="100vh"
      flex-dir="column"
      v-if="page==='profile'"
      v-bind:justify-content="mainBoxJustifyContent"
    >
      <CBox text-align="center"
            margin="0 auto">
        <CHeading text-align="center" pb="4">
          Профиль
        </CHeading>
          <CBox w="1080px" bg="gray.100" height="100vh" border="2px solid black" border-radius="24px">
          <CBox text-align="left" p="8">
            <CGrid
              h="300px"
              template-columns="repeat(8, 1fr)"
              gap="8"
            >
              <CGridItem col-span="2">
                <CAvatar size="2xl" name="Kelvin Dmereshone"/>
              </CGridItem>
              <CGridItem col-span="6">
                <CHeading>Developer</CHeading>
                <CText font-size="2xl">Konstantin Darlin</CText>
              </CGridItem>
            </CGrid>
          </CBox>
        </CBox>
      </CBox>
    </CBox>
  </div>
</template>

<script lang="js">
import {
  CBox,
  CButton,
  CAvatarGroup,
  CAvatar,
  CAvatarBadge,
  CModal,
  CModalContent,
  CModalOverlay,
  CModalHeader,
  CModalFooter,
  CModalBody,
  CModalCloseButton,
  CIconButton,
  CFlex,
  CHeading,
  CInput,
  CText,
  CStack,
  CGrid,
  CGridItem
} from '@chakra-ui/vue'
export default {
  name: 'IndexPage',
  components: {
    CBox,
    CButton,
    CAvatarGroup,
    CAvatar,
    CAvatarBadge,
    CModal,
    CModalContent,
    CModalOverlay,
    CModalHeader,
    CModalFooter,
    CModalBody,
    CModalCloseButton,
    CIconButton,
    CFlex,
    CHeading,
    CInput,
    CText,
    CStack,
    CGrid,
    CGridItem

  },
  inject: ['$chakraColorMode', '$toggleColorMode'],
  data () {
    return {
      showModal: false,
      isShowRegister: false,
      mainBoxJustifyContent: "center",
      page: "auth",
      isShowBackwardBtn: false,
      mainStyles: {
        dark: {
          bg: 'gray.700',
          color: 'whiteAlpha.900'
        },
        light: {
          bg: 'white',
          color: 'gray.900'
        }
      }
    }
  },
  computed: {
    colorMode () {
      return this.$chakraColorMode()
    },
    theme () {
      return this.$chakraTheme()
    },
    toggleColorMode () {
      return this.$toggleColorMode
    }
  },
  methods: {
    showToast () {
      this.$toast({
        title: 'Account created.',
        description: "We've created your account for you.",
        status: 'success',
        duration: 10000,
        isClosable: true
      })
    },
    auth () {
      this.page = "profile"
      this.mainBoxToUp()
      this.showBackwardBtn()
    },
    showRegister () {
      this.isShowRegister = !this.isShowRegister

      this.updateMainBoxJustifyContent()
    },
    toPreviousPage () {
      switch (this.page) {
        case "profile":
          this.page = "auth"
      }
      this.showBackwardBtn()
      this.updateMainBoxJustifyContent()
    },
    mainBoxToUp () {
      this.mainBoxJustifyContent = "flex-start"
    },
    mainBoxToCenter () {
      this.mainBoxJustifyContent = "center"
    },
    updateMainBoxJustifyContent () {
      if (this.isShowRegister) {
        this.mainBoxToUp()
      }
      else {
        this.mainBoxToCenter()
      }
    },
    showBackwardBtn () {
      console.log(this.page)
      this.isShowBackwardBtn =  true

      if (this.page==="auth") {
        this.isShowBackwardBtn = false
      }
    }
  },
  ready () {
    this.showBackwardBtn()
  }
}
</script>
