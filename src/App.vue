<template>
  <teleport to="head">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css"
          integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
  </teleport>

  <!-- start block-->
  <transition name="fade">
    <div class="start" v-if="!currencyRate">
      <div>
        <div class="d-flex">
          <img src="./assets/svg/keeper.svg" alt="keeper" class="big-icon m-3">
          <h2 class="m-1">Currency <br/> rate</h2>
        </div>
        <p class="text-warning" v-if="textError">{{ textError }}<br/>Пожалуйста, обновите страницу.</p>
      </div>
    </div>
  </transition>
  <!-- END start block-->

  <div class="container h-100 d-flex justify-content-center align-items-center">
    <div>
      <h1 class="text-white">Курс валют</h1>
      <p class="text-light">Курс 1 {{secondCurrency}} = {{ currencyRate }} {{ userCurrency }}. Обновлено {{ updateTime }}</p>
      <div class="d-sm-flex text-center justify-content-center align-items-center">
        <div class="input-group">
          <div class="input-group-prepend">
            <select class="form-control form-select-lg rounded-start rounded-0"
              v-model="userCurrency"
            >
              <option v-for="(el, key) in codeCurrencyMap"
                      :key="key"
                      :value="el"
              >
                {{el}}
              </option>
            </select>
          </div>
          <input
              type="number"
              class="form-control"
              v-model="userInput"
          >
        </div>
        <img src="./assets/svg/left-right.svg" alt="changer-icon" class="icon rotate-90 m-3">
        <div class="input-group">
          <div class="input-group-prepend">
            <select class="form-control form-select-lg rounded-start rounded-0"
                    v-model="secondCurrency"
            >
              <option v-for="(el, key) in codeCurrencyMap"
                      :key="key"
                      :value="el"
              >
                {{el}}
              </option>
            </select>
          </div>
          <input
              type="number"
              class="form-control"
              :value="userInput / currencyRate"
              readonly
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      codeCurrencyMap: {
        AD: 'EUR',
        AE: 'AED',
        AF: 'AFN',
        AG: 'XCD',
        AI: 'XCD',
        AL: 'ALL',
        AM: 'AMD',
        AO: 'AOA',
        AR: 'ARS',
        AS: 'USD',
        AT: 'EUR',
        AU: 'AUD',
        AW: 'AWG',
        AX: 'EUR',
        AZ: 'AZN',
        BA: 'BAM',
        BB: 'BBD',
        BD: 'BDT',
        BE: 'EUR',
        BF: 'XOF',
        BG: 'BGN',
        BH: 'BHD',
        BI: 'BIF',
        BJ: 'XOF',
        BL: 'EUR',
        BM: 'BMD',
        BN: 'BND',
        BO: 'BOB',
        BQ: 'USD',
        BR: 'BRL',
        BS: 'BSD',
        BT: 'BTN',
        BV: 'NOK',
        BW: 'BWP',
        BY: 'BYN',
        BZ: 'BZD',
        CA: 'CAD',
        CC: 'AUD',
        CD: 'CDF',
        CF: 'XAF',
        CG: 'CDF',
        CH: 'CHF',
        CI: 'XOF',
        CK: 'NZD',
        CL: 'CLP',
        CM: 'XAF',
        CN: 'CNY',
        CO: 'COP',
        CR: 'CRC',
        CU: 'CUC',
        CV: 'CVE',
        CW: 'ANG',
        CX: 'AUD',
        CY: 'EUR',
        CZ: 'CZK',
        DE: 'EUR',
        DJ: 'DJF',
        DK: 'DKK',
        DM: 'DOP',
        DO: 'DOP',
        DZ: 'DZD',
        EC: 'USD',
        EE: 'EUR',
        EG: 'EGP',
        EH: 'MAD',
        ER: 'ERN',
        ES: 'EUR',
        ET: 'ETB',
        FI: 'EUR',
        FJ: 'FJD',
        FM: 'USD',
        FO: 'DKK',
        FR: 'EUR',
        GA: 'XAF',
        GB: 'GBP',
        GD: 'XCD',
        GE: 'GEL',
        GF: 'EUR',
        GG: 'GBP',
        GH: 'GHS',
        GI: 'GIP',
        GL: 'DKK',
        GM: 'GMD',
        GN: 'GNF',
        GP: 'EUR',
        GQ: 'XAF',
        GR: 'EUR',
        GT: 'GTQ',
        GU: 'USD',
        GW: 'XOF',
        GY: 'GYD',
        HK: 'HKD',
        HN: 'HNL',
        HR: 'HRK',
        HT: 'HTG',
        HU: 'HUF',
        ID: 'IDR',
        IE: 'EUR',
        IL: 'ILS',
        IM: 'GBP',
        IN: 'INR',
        IO: 'USD',
        IQ: 'IQD',
        IR: 'IRR',
        IS: 'ISK',
        IT: 'EUR',
        JE: 'GBP',
        JM: 'JMD',
        JO: 'JOD',
        JP: 'JPY',
        KE: 'KES',
        KG: 'KGS',
        KH: 'KHR',
        KI: 'AUD',
        KM: 'KMF',
        KN: 'XCD',
        KP: 'KPW',
        KR: 'KRW',
        KW: 'KWD',
        KY: 'KYD',
        KZ: 'KZT',
        LB: 'LBP',
        LC: 'XCD',
        LI: 'CHF',
        LK: 'LKR',
        LR: 'LRD',
        LS: 'LSL',
        LT: 'EUR',
        LU: 'EUR',
        LV: 'EUR',
        LY: 'LYD',
        MA: 'MAD',
        MC: 'EUR',
        MD: 'MDL',
        ME: 'EUR',
        MF: 'EUR',
        MG: 'MGA',
        MH: 'USD',
        ML: 'XOF',
        MM: 'MMK',
        MN: 'MNT',
        MO: 'MOP',
        MP: 'USD',
        MQ: 'EUR',
        MR: 'MRU',
        MS: 'XCD',
        MT: 'EUR',
        MU: 'MUR',
        MV: 'MVR',
        MW: 'MWK',
        MX: 'MXN',
        MY: 'MYR',
        MZ: 'MZN',
        NA: 'NAD',
        NC: 'XPF',
        NE: 'NGN',
        NF: 'AUD',
        NG: 'NGN',
        NI: 'NIO',
        NL: 'EUR',
        NO: 'NOK',
        NP: 'NPR',
        NR: 'AUD',
        NU: 'NZD',
        NZ: 'NZD',
        OM: 'OMR',
        PA: 'PAB',
        PE: 'PEN',
        PF: 'XPF',
        PG: 'PGK',
        PH: 'PHP',
        PK: 'PKR',
        PL: 'PLN',
        PM: 'EUR',
        PN: 'NZD',
        PR: 'USD',
        PT: 'EUR',
        PW: 'USD',
        PY: 'PYG',
        QA: 'QAR',
        RE: 'EUR',
        RO: 'RON',
        RS: 'RSD',
        RU: 'RUB',
        RW: 'RWF',
        SA: 'SAR',
        SB: 'SBD',
        SC: 'SCR',
        SD: 'SDG',
        SE: 'SEK',
        SG: 'SGD',
        SH: 'SHP',
        SI: 'EUR',
        SJ: 'NOK',
        SK: 'EUR',
        SL: 'SLL',
        SM: 'EUR',
        SN: 'XOF',
        SO: 'SOS',
        SR: 'SRD',
        SS: 'SSP',
        ST: 'STN',
        SV: 'SVC',
        SX: 'ANG',
        SY: 'SYP',
        SZ: 'SZL',
        TC: 'USD',
        TD: 'XAF',
        TF: 'EUR',
        TG: 'XOF',
        TH: 'THB',
        TJ: 'TJS',
        TK: 'NZD',
        TL: 'USD',
        TM: 'TMT',
        TN: 'TND',
        TO: 'TOP',
        TR: 'TRY',
        TT: 'TTD',
        TV: 'AUD',
        TW: 'TWD',
        TZ: 'TZS',
        UA: 'UAH',
        UG: 'UGX',
        UM: 'USD',
        US: 'USD',
        UY: 'UYI',
        UZ: 'UZS',
        VA: 'EUR',
        VC: 'XCD',
        VE: 'VES',
        VG: 'USD',
        VI: 'USD',
        VN: 'VND',
        VU: 'VUV',
        WF: 'XPF',
        WS: 'USD',
        YE: 'YER',
        YT: 'EUR',
        ZA: 'ZAR',
        ZM: 'ZMW',
        ZW: 'ZWL',
        HM: 'AUD',
        FK: 'FKP',
        GS: 'GEL',
        LA: 'LAK',
        MK: 'MKD',
        AQ: 'USD',
        PS: 'ILS'
      },
      userCurrency: 'USD',
      secondCurrency: 'EUR',
      userInput: 100,
      currencyRate: null,
      updateTime: undefined,
      textError: ''
    }
  },
  methods: {
    async getCountryCode() {
      let countryCode;
      await fetch('https://get.geojs.io/v1/ip/country.json')
          .then(res => res.json())
          .then(json => {
            countryCode = json.country
            this.userCurrency = this.codeCurrencyMap[countryCode];
            this.textError = '';
          })
          .catch(error => {
            console.log('error getCountryCode', error)
            this.textError = 'Не удалось получить ваше положение.'
          });
    },
    async getCurrencyRate() {
      fetch(`https://api.apilayer.com/exchangerates_data/convert?to=${this.userCurrency}&from=${this.secondCurrency}&amount=1&apikey=nCRs5WMRksZ5bAvhgZPfuLG0P7QPxrzU`)
          .then(response => response.json())
          .then(json => {
            this.currencyRate = json.result;
            this.textError = '';
            let now = new Date();
            this.updateTime = `${now.getHours()}:${now.getMinutes()} ${now.getDate()}.${now.getMonth()}.${now.getFullYear()}`
          })
          .catch(error => {
            this.textError = 'Не удалось получить актуальный курс.'
            console.log('error getCurrencyRate', error)
          });
    }
  },
  watch: {
    userCurrency() {
      this.getCurrencyRate()
    },
    secondCurrency() {
      console.log('secondCurrency');
      this.getCurrencyRate()
    }
  },
  async mounted() {
    await this.getCountryCode();
    await this.getCurrencyRate();
  }
}
</script>

<style>
#app {
  height: 100vh;
  background: rgb(143, 0, 255);
  background: linear-gradient(135deg, rgba(143, 0, 255, 1) 0%, rgba(12, 1, 196, 1) 100%);
}

.big-icon {
  width: 60px;
  height: 60px;
}

.icon {
  width: 30px;
  height: 30px;
}

.start {
  background-color: #000;
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  z-index: 10;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* animation for start block*/
.fade-enter-active, .fade-leave-active {
  opacity: 1;
  visibility: visible;
  transition: all .5s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active до версии 2.1.8 */
{
  opacity: 0;
  visibility: hidden;
}

/* END animation for start block*/

.rotate-90 {
  transform: rotate(90deg);
}

input[readonly],
input[readonly]:focus{
  background-color: #ececec;
}
</style>
