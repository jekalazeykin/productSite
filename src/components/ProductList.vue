<template>
<div v-show="showProduct" class="product">
  <header class="header">
    <div class="header__login">
      <span>Hi, {{login}}</span> <button type="button" @click="logOut">Log out</button>
    </div>
    <div class="header__search">
      <input type="text" value="" placeholder="Search on the page" v-model="searchQuery">
      <button type="button" @click="search(body, searchQuery), makeMarkup(); showSearch= false">search</button>
    </div>
    <div class="header__main">
      <button type="button" @click="delateMarkup()">Main page</button>
    </div>
  </header>
  <main>
    <div class="searchView" @click="tieProducts">
      <div :class="showSearch?'firstSearch':'firstSearch-active'" @click="popupActivo4=true" vs-color="danger" vs-type="border"></div>
    </div>
    <div class="product-list">
      <div @click=" handleClick(index)" vs-color="danger" vs-type="border" v-for="(product, index) in products" v-show="showSearch" class="product-list__item">
        <img v-bind:id="product.title" v-bind:src="product.urlSmall" class="image" alt="">
        <p v-bind:id="product.title" class="title">{{product.title}} </p>
      </div>
    </div>
    <vs-popup fullscreen title="Details info" :active.sync="popupActivo4">
      <p v-if="popupActivo4" class="popup-title">{{currentProduct.title}}</p>
      <div class="product-list__popup">
        <img v-if="popupActivo4" v-bind:src="currentProduct.url" alt="">
        <p v-if="popupActivo4">{{currentProduct.description}}</p>
      </div>

    </vs-popup>
    <div v-show="showSearch" class="product-list__buttons">
      <button v-on:click="GetProductsMinus" v-show="counter>1">previus 10 products</button>
      <button v-on:click="GetProducts" v-show="counter<3">next 10 products</button>
    </div>
  </main>

</div>
</template>

<script>
export default {
  name: 'ProductList',
  data() {
    return {
      products: [],
      popupActivo4: false,
      currentProduct: null,
      counter: 0,
      showProduct: false,
      showSearch: true,
      showContainer: true,
      searchQuery: '',
      body: (document.getElementsByTagName('body'))[0],
      searchArr: [],
      login: '',
      id: ''
    }
  },
  methods: {
    GetProducts() {
      this.counter += 1;
      console.log(this.counter);
      if (this.counter === 1) {
        this.products = require('../json/first.json');
      } else if (this.counter === 2) {
        this.products = require('../json/second.json');
      } else if (this.counter === 3) {
        this.products = require('../json/third.json');
      }

      console.log(this.products)
    },
    GetProductsMinus() {
      this.counter -= 1;
      if (this.counter === 1) {
        this.products = require('../json/first.json');
      } else if (this.counter === 2) {
        this.products = require('../json/second.json');
      } else if (this.counter === 3) {
        this.products = require('../json/third.json');
      }
    },
    handleClick(index) {
      this.popupActivo4 = true;
      this.currentProduct = this.products[index];
      console.log(this.currentProduct);
    },
    showProductList() {
      if (!!localStorage.getItem('email') && !!localStorage.getItem('password')) {
        this.showProduct = true;
      }
    },
    logOut() {
      localStorage.removeItem('email');
      localStorage.removeItem('password');
      location.reload();
    },
    lookLogin() {
      this.login = localStorage.getItem('email');
    },
    search(where, what) {
      if (this.searchQuery != '') {
        what = what.toLowerCase();
        what = what[0].toUpperCase() + what.slice(1);
        let children = where.childNodes;
        for (let i = 0, l = children.length; i < l; i++) {
          let child = children[i],
            pos;
          if (child.nodeType == 3 && (pos = child.nodeValue.indexOf(what)) != -1) {
            let parent = child.parentNode;
            let mainParent = parent.parentNode;
            console.log(parent);
            console.log(mainParent.innerHTML);
            this.searchArr.push(mainParent.innerHTML);
            console.log(this.searchArr);
            return true;
          } else if (this.search(child, what)) {
            break;
          }
        }
        return false;
      }
    },
    makeMarkup() {
      let firstSearch = document.querySelector('.firstSearch');
      let searchView = document.querySelector('.searchView');
      this.searchArr.forEach(function(item, i, arr) {
        firstSearch.insertAdjacentHTML('beforeend', item);
      });

    },
    delateMarkup() {
      location.reload();
    },
    tieProducts(e) {
      let that = this;
      this.id = e.target.id;
      console.log(this.id);
      this.products.forEach(function(item, i, arr) {
        if (that.id == item.title) {
          that.currentProduct = item;
          console.log(that.currentProduct);
        }
      })
    }

  },
  created: function() {
    this.GetProducts();
    this.showProductList();
    this.lookLogin();
  }
}
</script>

<style lang="scss">
main {
    background-color: #d7e2f4;
}

.header {
    background: linear-gradient(to bottom, #1b62d6, #abb9d6);
    padding: 20px 15px;
    color: #fff;
    &__login {
        text-align: right;
        font-size: 20px;
        & > button {
            font-size: 60%;
            font-weight: 700;
            cursor: pointer;
            color: rgb(209,209,217);
            text-decoration: none;
            text-shadow: 0 -1px 2px rgba(0,0,0,.2);
            padding: 0.5em 1em;
            outline: none;
            border-radius: 3px;
            background: linear-gradient(rgb(110,112,120), rgb(81,81,86)) rgb(110,112,120);
            box-shadow: 0 1px rgba(255,255,255,.2) inset, 0 3px 5px rgba(0,1,6,.5), 0 0 1px 1px rgba(0,1,6,.2);
            transition: 0.2s ease-in-out;
            &:hover:not(:active) {
                background: linear-gradient(rgb(126,126,134), rgb(70,71,76)) rgb(126,126,134);
            }
        }
    }
    &__search {
        text-align: center;
        & > input {
            width: 320px;
            height: 40px;
            font-size: 25px;
            border-radius: 10px;
            padding-left: 40px;
        }
        & > button {
            color: white;
            font-size: 85%;
            font-weight: bold;
            text-decoration: none;
            text-shadow: -1px -1px #000;
            user-select: none;
            padding: 0.8em 2em;
            outline: none;
            background-color: #000;
            background-image: linear-gradient(45deg, rgba(255,255,255,.0) 30%, rgba(255,255,255,.8), rgba(255,255,255,.0) 70%), radial-gradient(190% 100% at 50% 0%, rgba(255,255,255,.7) 0%, rgba(255,255,255,.5) 50%, rgba(0,0,0,0) 50%);
            background-repeat: no-repeat;
            background-size: 200% 100%, auto;
            background-position: 200% 0, 0 0;
            border-radius: 10px;
            box-shadow: rgba(0,0,0,.3) 0 2px 5px;
            &:hover {
                transition: 0.5s linear;
                background-position: -200% 0, 0 0;
            }
        }
    }
    &__main {
        text-align: center;
        padding-top: 20px;
        & > button {
            height: 95px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 100%;
            font-weight: 700;
            color: #fff;
            text-shadow: #053852 -1px 1px, #053852 1px 1px, #053852 1px -1px, #053852 -1px -1px;
            text-decoration: none;
            user-select: none;
            padding: 0.3em 0.7em;
            outline: none;
            background: #053852 repeating-linear-gradient(135deg, #053852, #053852 10px, #1679ad 10px, #1679ad 20px, #053852 20px);
            box-shadow: inset -2px -2px rgba(0,0,0,.3), inset 2px 2px rgba(255,255,255,.3);
            transition: background-position 999999s, color 999999s, text-shadow 999999s;
            &:hover {
                opacity: 0.6;
            }
        }
    }
}

.product {
    z-index: 1;
    &-list {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        max-width: 1280px;
        &__item {
            text-align: center;
            width: 300px;
            cursor: pointer;
            margin: 20px;
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
            border-radius: 5px;
            padding: 15px 5px;
            background-color: #fff;
            &:hover {
                transform: scale(1.15);
                z-index: 10;
            }
        }
        &__buttons {
            text-align: center;
            padding: 30px 0 80px;
            & > button {
                color: white;
                text-decoration: none;
                padding: 0.5em 2em;
                outline: none;
                border-width: 2px 0;
                border-style: solid none;
                border-color: #FDBE33 #000 #D77206;
                border-radius: 6px;
                background: linear-gradient(#F3AE0F, #E38916) #E38916;
                transition: 0.2s;
                height: 45px;
                font-size: 120%;
                &:hover {
                    background: linear-gradient(#f5ae00, #f59500) #f5ae00;
                }
            }
        }
        &__popup {
            display: flex;
            @media only screen and (max-width: 768px) {
                display: block;
            }
            & > p {
                padding: 20px 15px;
                font-size: 20px;
            }
            & > img {
                @media only screen and (max-width: 420px) {
                    width: 85vw;
                }
            }
        }
    }
}

.popup-title {
    font-size: 1.6em;
    font-weight: 800;
}

.firstSearch {
    text-align: center;
    padding: 30px 0 60px;
    cursor: pointer;

}
.firstSearch-active {
    text-align: center;
    padding: 30px 0 60px;
    cursor: pointer;
    height: 170vh;
}
.title {
    font-size: 1.2em;
    font-weight: 600;
}
.vs-popup-text {
    }
</style>
