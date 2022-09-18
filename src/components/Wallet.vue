<template>
  <div class="collection-tabs">
    <div class="tabs">
      <div class="container">

        <div class="tab" @click="changeActiveTab('mumbai')" :class="{ '-active':  selectedTab === 'mumbai'}">
          Mumbai
        </div>

        <div class="tab" @click="changeActiveTab('rinkeby')" :class="{ '-active':  selectedTab === 'rinkeby'}">
          Rinkeby
        </div>

      </div>
    </div>
  </div>

  <div class="content">
    <section class="section">
      <span class="section-header">Wallet NFTs</span>

      <div class="card-block">

        <div class="card" v-for="item in walletNFTS">
          <div class="image">
            <img v-if="item.metadata && item.metadata.image" :src="item.metadata.image" alt="">
          </div>
          <div class="content">
            <div class="name">
              {{ item.name }} #{{ item.token_id }}
            </div>
          </div>
        </div>

      </div>

    </section>
  </div>
</template>

<script setup>
defineProps({
  address: {
    type: String,
    required: true,
  },
});
</script>

<script>
import axios from "axios";
import Env from "../../env";

export default {
  name: "Wallet",
  data() {
    return {
      selectedTab: 'mumbai',
      walletNFTS: [],
    }
  },
  methods: {
    changeActiveTab(tab) {
      this.selectedTab = tab

      this.load()
    },
    load() {
      console.log(this.address)
      axios
          .get('https://deep-index.moralis.io/api/v2/' + this.address + '/nft?chain=' + this.selectedTab, {
            'headers': {
              'X-API-Key': Env.API_KEY
            }
          })
          .then(response => {
            // console.log('response', response)
            if (response.data && response.data.result && response.data.result.length) {
              this.walletNFTS = response.data.result.map((item) => {
                item.metadata = JSON.parse(item.metadata);
                // console.log('item.metadata', item.metadata)
                return item;
              })
            }
          })
    },
  },
  mounted() {
    this.load()
  }
}
</script>

<style lang="less">
.collection-tabs {
  > .tabs {
    > .container {
      display: flex;
      justify-content: center;
      align-items: center;
      width: var(--main-width-full);
      margin: 0 auto;
      border-bottom: 1px solid var(--dark-blue-10);

      > .tab {
        position: relative;
        margin: 0 15px;
        padding-bottom: 8px;
        font-weight: 600;
        font-size: 16px;
        line-height: 23px;
        color: var(--dark-blue);
        cursor: pointer;
        transition: .2s;

        &.-active {
          //text-decoration: underline;

          &::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            display: block;
            width: 100%;
            height: 2px;
            background-color: #000;
          }
        }
      }
    }
  }

  > .content {
    > * {
      margin-top: 100px;

      > .content {
        padding-top: 0px;
      }
    }
  }
}

.section {
  margin-top: 40px;

  .section-header {
    font-size: 16px;
    font-weight: bold;
  }

  .card-block {
    margin-top: 10px;
    display: flex;
    flex-wrap: wrap;
    column-gap: 18px;
    row-gap: 25px;

    .card {
      box-shadow: 0 1px 2px rgb(0 26 84 / 20%);

      .image {
        border-radius: 20px 20px 0 0;
        height: 280px;
        overflow: hidden;
        position: relative;
        width: 280px;

        img {
          border-radius: 0 0 20px 20px;
          height: 100%;
          object-fit: cover;
          width: 100%;
          border-style: none;
        }
      }

      .content {
        background-color: var(--white);
        border-radius: 0 0 20px 20px;
        color: var(--dark-blue);
        min-height: 104px;
        padding: 15px 20px;
        width: 280px;

        .name {
          font-size: 16px;
          font-weight: 700;
          line-height: 20px;
        }
      }
    }
  }
}

@media (max-width: 1023px) {
  .collection-tabs {
    > .tabs {
      > .container {
        > .tab {
          margin: 0 10px;
          padding-bottom: 10px;
          font-size: 14px;
          line-height: 20px;
        }
      }
    }
  }
}

@media (max-width: 767px) {
  .collection-tabs {
    > .tabs {
      margin-bottom: 0px;
      padding-top: 20px;
      background-color: var(--blue-gray);

      > .container {
        > .tab {
          margin: 0 7px;
          padding-bottom: 12px;
          font-size: 13px;
          line-height: 18px;
        }
      }
    }
  }
}
</style>