<template>
    <div>
        <div v-if="loading==false">
            <app-nav v-bind:menu-links="menuLinks"></app-nav>
            <transition name="fade">
                <router-view :page-list="pages" :load-check="loading"></router-view>
            </transition> 
        </div> 
        <div v-else>
            <transition name="fade">
                <div class="uk-container uk-container-expand loading-animation">
                      <svg width="80" height="60" viewBox="5 0 80 60"><path class="wave" fill="none" stroke="#fff" stroke-width="4" stroke-linecap="round" d="M 0 37.5 c 7.684299348848887 0 7.172012725592294 -15 15 -15 s 7.172012725592294 15 15 15 s 7.172012725592294 -15 15 -15 s 7.172012725592294 15 15 15 s 7.172012725592294 -15 15 -15 s 7.172012725592294 15 15 15 s 7.172012725592294 -15 15 -15 s 7.172012725592294 15 15 15 s 7.172012725592294 -15 15 -15 s 7.172012725592294 15 15 15 s 7.172012725592294 -15 15 -15 s 7.172012725592294 15 15 15 s 7.172012725592294 -15 15 -15 s 7.172012725592294 15 15 15 s 7.172012725592294 -15 15 -15" />
                        </svg>
                </div>
            </transition>  
        </div>
    </div>
</template>

<script>
    import es6Promise from 'es6-promise';
    es6Promise.polyfill();
    import 'es6-promise/auto'
    import axios from 'axios';
	import Nav from './components/Nav.vue';
    export default {
        components: {
            appNav: Nav
        },
        data: function () {
            return {
                loading: true,
                menuLinks: [],
                menuColor: "",
                pages: [],
                homePage: [],
                aboutPage: []    
            }
        },
        beforeMount() {
            // this.getBrowser();
            this.getMenu();
            this.getPages();
        },
        mounted: function() {
            // this.isLoaded();
        },
        watch: {
            loading: function (newLoading) {
                this.loading = newLoading;
            }
        },
        computed: {
            isLoaded: function() {
                if (this.hasOwnProperty('pages')
                    && this.pages.length > 0) {
                    // console.log('LOADED');
                    return true;
                }
                // console.log('NOT LOADED');
                return false;
            }
        },
        methods: {
            getMenu: function() {
              var app = this
            // axios.get('http://dev.hypenotic.com/cms-empire/wp-json/wp-api-menus/v2/menus/2')
            axios.get('http://cms.empirestateconnector.com/wp-json/wp-api-menus/v2/menus/2')
              .then(function (response) {
                app.menuLinks = response.data.items;
                // console.log(response.data);
              })
              .catch(function (error) {
                console.log(error)
              })
            },
            getPages: function() {
              var app = this
            //   axios.get('http://dev.hypenotic.com/cms-empire/wp-json/wp/v2/pages?_embed')
            axios.get('http://cms.empirestateconnector.com/wp-json/wp/v2/pages?_embed')
              .then(function (response) {
                app.pages = response.data;
                for (let page of response.data) {
                    // console.log(page.slug);
                    if (page.slug == 'home') {
                        // console.log('found it');
                        app.homePage = page;
                        break;
                    }
                };
                setTimeout(function(){ app.loading = false }, 1000);
                
                // this.isLoaded();
              })
              .catch(function (error) {
                console.log(error)
              })
            }
        }
    }
</script>

<style lang="scss">

// ===== GLOBAL
@import './sass/variables.scss';

*, :after, :before {
    box-sizing: border-box;
}

html, body {
    height: 100%;
    // font-family: serif;
    font-family: $body-type;
    font-size: 18px;
    font-weight: 300;
    // line-height: 1.5rem;
    @media #{$medium-and-up} {
        font-size: 24px;
        line-height: 1.5rem;
    }
}

h1,h2,h3,h4,h5,h6 {
    font-family: $heading-type;
    color: #666;
}

h1 {

}

h2 {
    font-size: 1.3rem;
    line-height: 1.6rem;
}

a {
    color: $main-accent;
    // font-size: 0.6rem !important;
    // line-height: 0.8rem !important;
    &:hover {
        color: lighten($main-accent, 5);
    }
}



body {
    margin: 0;
}

body.noscroll {
    overflow: none;
}

blockquote {
    padding-left: 24px;
    margin-left: 24px;
    border-left: 1px solid #e3e3e3;
    color: #666;
}

/*
::-webkit-scrollbar {
    width: 10px;
}
*/
    
// TYPE
.main-banner-content {
    color: white;
    position: relative;
    z-index: 500;
    display: inline-block;
    max-width: 960px;
    margin: 0 auto;
    // text-align: center;
    @media #{$large-and-up} {
        max-width: 1000px;
        width: 1000px;
        padding-left: 100px; 
        // display: block;
        // width: 1200px;
        // margin: 0 auto;
    }
    @media #{$xlarge-and-up} {
        display: block;
        width: 960px;
        margin: 0 auto;
    }
    h1, h2 {
        color: white;
    }
    h1 {
        /* position: relative; */
        font-size: 10vw;
        line-height: 11vw;
        padding-top: 130px;
        text-transform: uppercase;
        @media #{$medium-and-up} {
            font-size: 6vw;
            line-height: 6vw;
            padding-top: 180px;
        }
        @media #{$xlarge-and-up} {
            font-size: 64px;
            line-height: 64px;
            padding-top: 200px;
        }
    }
    h2 {
        margin-top: 10px;
        width: 80%;
        font-weight: 300;
        line-height: 130%;
        font-family: $body-type;
        font-weight: 200;
        @media #{$small-and-down} {
            width: 95%;
            font-size: 5.5vw;
            line-height: 7vw;
        }
    }
    h2 p {
        margin-top: 10px;
        font-weight: 300;
        line-height: 130%;
        font-family: $body-type;
        font-weight: 200;
        @media #{$small-and-down} {
            width: 95%;
            font-size: 5.5vw;
            line-height: 7vw;
        }
    }
    .contentItalic,
    em {
        color: $white;
    }
}

.contentItalic,
em {
    color: #666;
}

.overview,
.overview p {    
    @media #{$medium-and-up} {
        font-size: 32px;
        line-height: 40px;
	}
}

// LAYOUT

.view {
    position: relative;
    z-index: 400;
}

.view--padding-top {
    padding-top: 100px;
}

.nav-container {
    z-index: 500;
    position: fixed;
    width: 95%;
    margin: 0 2.5%;
    @media #{$large-and-up} {
        position: absolute;
        width: 92%;
        margin: 0 4%;
    }
    @media #{$xlarge-and-up} {
        width: 92%;
        // display: flex;
        // justify-content: space-between;
        // position: absolute;
        // width: 70% !important;
        margin: 0;
        // position: relative;
    }
}

.non-home-container {
    margin-top: 40px !important;
}

// TRANSITIONS
.fade-enter-active, .fade-leave-active {
  transition-property: opacity;
  transition-duration: .25s;
}

.fade-enter-active {
  transition-delay: .2s;
}

.fade-enter, .fade-leave-active {
  opacity: 0
}

// LOADING ANIMATION
.loading-animation {
    // width: 100vw;
    // height: 100vh;
    // position: absolute;
    // background: white;
    background: $main-accent;
    text-align: center;
    padding-top: 45vh; 
    min-height: 60vh;
}

.loading-animation.loading-animation--page {
    min-height: 70vh;
    padding-top: 10%; 
    background: $white;
}

.wave {
  animation: moveTheWave 2400ms linear infinite;
  stroke-dasharray: 0 16 101 16;
}

@keyframes moveTheWave {
  0% {
    stroke-dashoffset: 0;
    transform: translate3d(0, 0, 0);
  }
  100% {
    stroke-dashoffset: -133;
    transform: translate3d(-90px, 0, 0);
  }
}

// Third Party Component
.social-share-buttons {
    span {
        margin-left: 16px;
        display: inline-block;
        padding-top: 5px;
        @media #{$small-and-down} {
            margin-right: 16px;
            margin-left: 3px;
        }
        i {
             display: inline-block;
             margin-right: 3px;
        }
        &:hover {
            color: $main-accent;
            cursor: pointer;
        }
    }
}

</style>
