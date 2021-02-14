<template>
  <v-container>
    <v-row class="text-left justify-center">
      <div class="width-600">
        <h3>
          Calculator
        </h3>
        <p>
          Default values do not imply return expectations. Change them up as necessary.
        </p>
        <v-row>
          <v-col cols="7">
            <v-row>
              <v-col cols="12" md="6" sm="6" xs="12" class="mobile-px-0">
                <vuetify-money
                  flat
                  solo
                  v-model="kcsStacked"
                  class="input-color-green"
                  label="KCS stacked"
                  :outlined="false"
                  :options="kcsStackedOption"
                />
              </v-col>
              <v-col cols="12" md="6" sm="6" xs="12" class="mobile-px-0">
                <vuetify-money
                  v-model="kcsOwn"
                  class="input-color-green"
                  label="KCS you own"
                  :outlined="false"
                  :options="kcsOwnOption"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" md="6" sm="6" xs="12" class="mobile-px-0">
                <vuetify-money
                  v-model="dailyVolume"
                  class="input-color-green"
                  label="Daily trading volume"
                  :outlined="false"
                  :options="suffixUSD"
                />
              </v-col>
              <v-col cols="12" md="6" sm="6" xs="12" class="mobile-px-0">
                <div class="input-color-green average-fee">
                  <label>Average Trading Fee</label>
                  <input type="number" v-model="averageTradingFee" />
                  <span> % </span>
                </div>
              </v-col>
            </v-row>
            <v-row class="show-desktop">
              <v-col cols="12">
                <v-row>
                  <v-col cols="6" md="6" sm="6" xs="6">
                    <vuetify-money
                      v-model="tradingFeeCollected"
                      class="input-color-green"
                      label="Trading fees collected"
                      :outlined="false"
                      :options="suffixUSD"
                      :readonly="true"
                    />
                  </v-col>
                  <v-col cols="6" md="6" sm="6" xs="6" class="d-flex align-center">
                    <v-btn
                      color="success"
                    >
                      Sign Up Now
                    </v-btn>
                  </v-col>
                </v-row>
              </v-col>
            </v-row>
            <v-row class="show-desktop">
              <v-col cols="12">
                <v-row>
                  <v-col cols="12">
                    <v-btn
                      color="default"
                      small
                      @click="initValues"
                    >
                      Reset Values
                    </v-btn>
                  </v-col>
                </v-row>
              </v-col>
            </v-row>
          </v-col>
          <v-col cols="5">
            <h4>
              Expected Rewards
            </h4>
            <v-spacer />
            <h5>
              $
              <span> {{rewardDay}} </span><br/>
              <span class="date-opacity-7"> per day </span>
            </h5>
            <v-spacer />
            <h5>
              $
              <span> {{rewardMonth}} </span><br/>
              <span class="date-opacity-7"> per month </span>
            </h5>
            <v-spacer />
            <h5>
              $
              <span> {{rewardYear}} </span><br/>
              <span class="date-opacity-7"> per year </span>
            </h5>
          </v-col>
        </v-row>
        <v-row class="show-mobile">
          <v-col cols="12">
            <v-row>
              <v-col cols="6" md="6" sm="6" xs="6">
                <vuetify-money
                  v-model="tradingFeeCollected"
                  class="input-color-green"
                  label="Trading fees collected"
                  :outlined="false"
                  :options="suffixUSD"
                  :readonly="true"
                />
              </v-col>
              <v-col cols="6" md="6" sm="6" xs="6" class="d-flex align-center">
                <v-btn
                  color="success"
                >
                  Sign Up Now
                </v-btn>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        <v-row class="show-mobile">
          <v-col cols="12" class="show-mobile">
            <v-row>
              <v-col cols="12">
                <v-btn
                  color="default"
                  small
                  @click="initValues"
                >
                  Reset Values
                </v-btn>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </div>
    </v-row>
  </v-container>
</template>

<script>
  import axios from 'axios'
  import dailyVolumeUrl from '@/utils/index'

  export default {
    name: 'Calculator',

    data: () => ({
      kcsStackedOption: {
        suffix: "( KCS )",
        precision: 0,
      },
      kcsOwnOption: {
        suffix: "( KCS )",
        precision: 0,
      },
      suffixPercent: {
        suffix: "%",
        precision: 3,
      },
      suffixUSD: {
        prefix: "$",
        precision: 0,
      },
      kcsStacked: 80118638,
      kcsOwn: 10000,
      dailyVolume: 1000000000,
      averageTradingFee: 0.1,
      apiVal: 0
    }),

    computed: {
      payoutPerDay() {
        return 0.10 * this.dailyVolume / 100 / 2 / this.kcsStacked
      },
      
      tradingFeeCollected() {
        return this.dailyVolume / 100 * this.averageTradingFee
      },

      rewardDay() {
        return parseFloat((this.payoutPerDay * this.kcsOwn * this.apiVal).toFixed(4))
      },

      rewardMonth() {
        return parseFloat((this.rewardDay * 30).toFixed(4))
      },

      rewardYear() {
        return parseFloat((this.rewardDay * 365).toFixed(2))
      }
    },

    mounted() {
      try {
        axios.get(dailyVolumeUrl).then(res => {
          if (res.status === 200) {
            this.apiVal = res.data['kucoin-shares'].usd
          }
        })
      } catch (e) {
        console.log(e)
      }
    },

    methods: {
      initValues() {
        this.kcsStacked = 80118638
        this.kcsOwn = 10000
        this.averageTradingFee = 0.1
        this.dailyVolume = 1000000000
      }
    }
  }
</script>

<style scopped>
label {
  font-size: 12px;
  color: rgb(0 0 0 / 70%);
}

.input-color-green input {
  caret-color: #4caf50;
}

.input-color-green input:focus {
  color: #4caf50 !important;
  cursor: pointer;
}

.input-color-green input:hover {
  color: #4caf50 !important;
  cursor: pointer;
}

.input-color-green .v-input__slot::after {
  border: none !important;
}

.input-color-green .v-input__slot::before {
  border: none !important;
}

.input-color-green label {
  color: rgb(0 0 0 / 70%) !important;
}

.input-color-green .v-text-field__prefix {
  color: rgb(0 0 0 / 70%) !important;
}

.input-color-green .v-text-field__suffix {
  color: rgb(0 0 0 / 70%) !important;
}

h5 {
  margin-top: 1rem;
  font-size: 18px;
  opacity: .8;
  font-weight: 500;
}

.date-opacity-7 {
  opacity: .5;
  font-weight: 400;
  font-size: 18px;
  color: #000000de;
}

.width-600 {
  max-width: 600px;
  margin: 1rem;
}

@media only screen and (max-width: 768px) {
  .mobile-px-0 {
    padding-top: 0 !important;
    padding-bottom: 0 !important;
  }

  .show-desktop {
    display: none !important;
  }

  .show-mobile {
    display: flex !important;
  }
}

@media only screen and (min-width: 768px) {
  .show-desktop {
    display: flex !important;
  }

  .show-mobile {
    display: none !important;
  }
}

p {
  font-size: 12px;
  margin-bottom: 40px !important;
}

.average-fee {
  position: relative;
}
.average-fee input {
  outline: none;
  width: 100%;
}
.average-fee span {
    position: absolute;
    top: 24px;
    right: 0;
    background: white;
    width: 30px;
    height: 25px;
}
</style>
