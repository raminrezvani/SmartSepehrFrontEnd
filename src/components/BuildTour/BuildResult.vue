<template>
  <section>
    <div class="row">
      <!-- --- FILTER --- -->
      <div class="col-12 col-md-3">
        <div class="sticky-top">
          <div class="row align-items-center bg-white py-3 rounded">
            <!-- --- NAME --- -->
            <div class="col-12">
              <label for="filter_name" class="text-muted mb-1">هتل</label>
              <v-select multiple :options="hotel_names" v-model="filter.hotel"></v-select>
            </div>
            <!-- --- SORT --- -->
            <div class="col-12 mt-3">
              <label for="order_by_filter" class="text-muted mb-1">مرتب سازی</label>
              <select class="form-select" id="order_by_filter">
                <option value="price_a">قیمت - کم به زیاد</option>
                <option value="price_d">قیمت - زیاد به کم</option>
                <option value="hour_d_a">ساعت پرواز - صبح به شب</option>
                <option value="hour_d_d">ساعت پرواز - شب به صبح</option>
              </select>
            </div>
            <!-- --- ANALYSE --- -->
            <div class="col-12 mt-3">
              <button class="btn btn-primary" :disabled="analysis_loading" v-on:click="show_analysis = !show_analysis">
                <i class="fa fa-spinner fa-spin ms-1" v-if="analysis_loading"></i>
                <span>انالیز هتلی</span>
              </button>
            </div>
            <build-tour-analyse v-if="show_analysis" @closeAnalysis="closeAnalysis"
                                :data="analysis_data" :adults="adults"></build-tour-analyse>
          </div>
          <!-- --- LAST UPDATE --- -->
          <div class="row mt-3 bg-white rounded p-3 align-items-center">
            <div class="col-6">
              <p class="m-0">آخرین بروزرسانی : </p>
            </div>
            <div class="col-5">
              <p class="m-0">{{ last_search.minute }} {{ last_search.text }} پیش</p>
            </div>
            <div class="col-1 text-danger" v-if="last_search.warning">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                   class="bi bi-exclamation-circle" viewBox="0 0 16 16">
                <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                <path
                    d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 4.995z"/>
              </svg>
            </div>
          </div>
        </div>
      </div>
      <!-- --- RESULT --- -->
      <div class="col-12 col-md-9 mt-3 mt-md-0">
        <div class="alert alert-warning text-center" v-if="!go_flight.length">
          <h5 class="m-0">پرواز رفت یافت نشد</h5>
        </div>
        <div class="alert alert-warning text-center" v-if="!return_flight.length">
          <h5 class="m-0">پرواز برگشت یافت نشد</h5>
        </div>
        <div class="alert alert-warning text-center" v-if="!hotels.length">
          <h5 class="m-0">هتل یافت نشد</h5>
        </div>
        <main>
          <build-component v-for="(tour, index) in hotels" :key="index" :index="index" :hotel="tour"
                           :adults="adults" :target="target" :source="source"
                           :analysis_data="analysis_data"
                           :analysis_loading="analysis_loading"
                           :go_flight="go_flight[go_flight_index]"
                           :go_flight_provider="go_flight_provider_index"
                           :return_flight="return_flight[return_flight_index]"
                           :return_flight_provider="return_flight_provider_index"></build-component>
        </main>
      </div>
    </div>
    <!-- --- GO FLIGHT MODAL --- -->
    <div class="modal modal-xl fade" id="build_tour_go_flight_modal" tabindex="-1"
         aria-labelledby="build_tour_go_flight_modal"
         aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">پرواز رفت</h1>
          </div>
          <div class="modal-body">
            <div v-for="(flight, index) in go_flight"
                 :key="index">
              <section :class="{'border rounded p-3 shadow-xs mt-3': true, 'flight-active': go_flight_index === index}">
                <div :class="{'row align-items-center': true}">
                  <!-- AIRLINE NAME -->
                  <div class="col-12 col-md-2">
                    <h5>{{ flight.airline_name }}</h5>
                    <h5 class="mt-2">{{ flight.flight_number }}</h5>
                  </div>
                  <!-- GO FLIGHT -->
                  <div class="col-5 col-md-3 mt-3 mt-md-0 text-center">
                    <h5 class="mt-2">{{ flight.go_time }}</h5>
                    <h5 class="mt-2">{{ flight.go_date }}</h5>
                  </div>
                  <!-- ICON -->
                  <div class="col-2 col-md-1 mt-3 mt-md-0 text-center">
                    <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor"
                         class="bi bi-airplane rotate-left" viewBox="0 0 16 16" data-v-12fb0470="">
                      <path
                          d="M6.428 1.151C6.708.591 7.213 0 8 0s1.292.592 1.572 1.151C9.861 1.73 10 2.431 10 3v3.691l5.17 2.585a1.5 1.5 0 0 1 .83 1.342V12a.5.5 0 0 1-.582.493l-5.507-.918-.375 2.253 1.318 1.318A.5.5 0 0 1 10.5 16h-5a.5.5 0 0 1-.354-.854l1.319-1.318-.376-2.253-5.507.918A.5.5 0 0 1 0 12v-1.382a1.5 1.5 0 0 1 .83-1.342L6 6.691V3c0-.568.14-1.271.428-1.849Zm.894.448C7.111 2.02 7 2.569 7 3v4a.5.5 0 0 1-.276.447l-5.448 2.724a.5.5 0 0 0-.276.447v.792l5.418-.903a.5.5 0 0 1 .575.41l.5 3a.5.5 0 0 1-.14.437L6.708 15h2.586l-.647-.646a.5.5 0 0 1-.14-.436l.5-3a.5.5 0 0 1 .576-.411L15 11.41v-.792a.5.5 0 0 0-.276-.447L9.276 7.447A.5.5 0 0 1 9 7V3c0-.432-.11-.979-.322-1.401C8.458 1.159 8.213 1 8 1c-.213 0-.458.158-.678.599Z"
                          data-v-12fb0470=""></path>
                    </svg>
                  </div>
                  <!-- RETURN FLIGHT -->
                  <div class="col-5 col-md-3 mt-3 mt-md-0 text-center">
                    <h5 class="mt-2">{{ flight.return_time }}</h5>
                    <h5 class="mt-2">{{ flight?.return_date }}</h5>
                  </div>
                  <!-- PRICE -->
                  <div class="col-12 col-md-3 mt-3 mt-md-0">
                    <div class="d-flex algin-items-center justify-content-between">
                      <h5>هر نفر:</h5>
                      <h5 class="text-center">{{ Number(flight.min_price).toLocaleString() }} تومان</h5>
                    </div>
                    <div class="mt-3 d-flex justify-content-between align-items-center">
                      <h5 class="text-center text-danger" v-if="calcGoFlightDif(flight.min_price) > 0">
                        <span>{{ Number(Math.abs(calcGoFlightDif(flight.min_price))).toLocaleString() }}</span>
                        <i class="fa fa-angle-up me-1"></i>
                      </h5>
                      <h5 class="text-center" v-else-if="calcGoFlightDif(flight.min_price) === 0">
                        <span>0</span>
                      </h5>
                      <h5 class="text-center text-success" v-else>
                        <span>{{ Number(Math.abs(calcGoFlightDif(flight.min_price))).toLocaleString() }}</span>
                        <i class="fa fa-angle-down me-1"></i>
                      </h5>
                      <button class="btn btn-primary w-50 " data-bs-toggle="collapse"
                              type="button" aria-expanded="false" v-on:click="showMoreGo(index)">
                        سایر ({{ flight.providers.length }})
                      </button>
                    </div>
                  </div>
                </div>
              </section>
              <div class="collapse show fade active" :id="`go_flight_modal_collapse_${index}`"
                   v-if="show_more_go_flight_index === index">
                <div :class="{'row': true}">
                  <div
                      :class="{'col-12 col-md-4 mt-4': true}"
                      v-for="(provider, providerIndex) in flight.providers" :key="providerIndex">
                    <div
                        :class="{'border p-3 rounded': true, 'flight-active': go_flight_index === index && providerIndex === go_flight_provider_index}">
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>{{ provider.provider_name }}</h5>
                        <img :src="provider.provider_logo" :alt="provider.provider_name" width="35" height="35">
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>{{ provider.go_date }}</h5>
                        <h5 class="mt-2">{{ provider.go_time }}</h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>{{ provider?.return_date }}</h5>
                        <h5 class="mt-2">{{ provider?.return_time }}</h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>تعداد صندلی</h5>
                        <h5 class="mt-2">{{ provider.seat }}</h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center mt-2">
                        <h5>هر نفر :</h5>
                        <h5>
                          <span>{{ Number(provider.price).toLocaleString() }}</span>
                          <span> تومان </span>
                        </h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center mt-2">
                        <h5>قیمت کل :</h5>
                        <h5>
                          <span>{{ Number(provider.price * adults).toLocaleString() }}</span>
                          <span> تومان </span>
                        </h5>
                      </div>
                      <hr>
                      <div class="">
                        <button v-if="go_flight_index === index && providerIndex === go_flight_provider_index"
                                class="btn btn-success w-100">انتخاب شده
                        </button>
                        <button v-on:click="setGoFlight(index, providerIndex)" class="btn btn-success w-100" v-else>
                          انتخاب
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- --- RETURN FLIGHT MODAL --- -->
    <div class="modal modal-xl fade" id="build_tour_return_flight_modal" tabindex="-1"
         aria-labelledby="build_tour_return_flight_modal"
         aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">پرواز برگشت</h1>
          </div>
          <div class="modal-body">
            <div v-for="(flight, index) in return_flight"
                 :key="index">
              <section
                  :class="{'border rounded p-3 shadow-xs mt-3': true, 'flight-active': return_flight_index === index}">
                <div :class="{'row align-items-center': true}">
                  <!-- AIRLINE NAME -->
                  <div class="col-12 col-md-2">
                    <h5>{{ flight.airline_name }}</h5>
                    <h5 class="mt-2">{{ flight.flight_number }}</h5>
                  </div>
                  <!-- GO FLIGHT -->
                  <div class="col-5 col-md-3 mt-3 mt-md-0 text-center">
                    <h5 class="mt-2">{{ flight.go_time }}</h5>
                    <h5 class="mt-2">{{ flight.go_date }}</h5>
                  </div>
                  <!-- ICON -->
                  <div class="col-2 col-md-1 mt-3 mt-md-0 text-center">
                    <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor"
                         class="bi bi-airplane rotate-left" viewBox="0 0 16 16" data-v-12fb0470="">
                      <path
                          d="M6.428 1.151C6.708.591 7.213 0 8 0s1.292.592 1.572 1.151C9.861 1.73 10 2.431 10 3v3.691l5.17 2.585a1.5 1.5 0 0 1 .83 1.342V12a.5.5 0 0 1-.582.493l-5.507-.918-.375 2.253 1.318 1.318A.5.5 0 0 1 10.5 16h-5a.5.5 0 0 1-.354-.854l1.319-1.318-.376-2.253-5.507.918A.5.5 0 0 1 0 12v-1.382a1.5 1.5 0 0 1 .83-1.342L6 6.691V3c0-.568.14-1.271.428-1.849Zm.894.448C7.111 2.02 7 2.569 7 3v4a.5.5 0 0 1-.276.447l-5.448 2.724a.5.5 0 0 0-.276.447v.792l5.418-.903a.5.5 0 0 1 .575.41l.5 3a.5.5 0 0 1-.14.437L6.708 15h2.586l-.647-.646a.5.5 0 0 1-.14-.436l.5-3a.5.5 0 0 1 .576-.411L15 11.41v-.792a.5.5 0 0 0-.276-.447L9.276 7.447A.5.5 0 0 1 9 7V3c0-.432-.11-.979-.322-1.401C8.458 1.159 8.213 1 8 1c-.213 0-.458.158-.678.599Z"
                          data-v-12fb0470=""></path>
                    </svg>
                  </div>
                  <!-- RETURN FLIGHT -->
                  <div class="col-5 col-md-3 mt-3 mt-md-0 text-center">
                    <h5 class="mt-2">{{ flight.return_time }}</h5>
                    <h5 class="mt-2">{{ flight?.return_date }}</h5>
                  </div>
                  <!-- PRICE -->
                  <div class="col-12 col-md-3 mt-3 mt-md-0">
                    <div class="d-flex algin-items-center justify-content-between">
                      <h5>هر نفر:</h5>
                      <h5 class="text-center">{{ Number(flight.min_price).toLocaleString() }} تومان</h5>
                    </div>
                    <div class="mt-3 d-flex justify-content-between align-items-center">
                      <h5 class="text-center text-danger" v-if="calcReturnFlightDif(flight.min_price) > 0">
                        <span>{{ Number(Math.abs(calcReturnFlightDif(flight.min_price))).toLocaleString() }}</span>
                        <i class="fa fa-angle-up me-1"></i>
                      </h5>
                      <h5 class="text-center" v-else-if="calcReturnFlightDif(flight.min_price) === 0">
                        <span>0</span>
                      </h5>
                      <h5 class="text-center text-success" v-else>
                        <span>{{ Number(Math.abs(calcReturnFlightDif(flight.min_price))).toLocaleString() }}</span>
                        <i class="fa fa-angle-down me-1"></i>
                      </h5>
                      <button class="btn btn-primary w-50 " data-bs-toggle="collapse"
                              type="button" aria-expanded="false" v-on:click="showMoreReturn(index)">
                        سایر ({{ flight.providers.length }})
                      </button>
                    </div>
                  </div>
                </div>
              </section>
              <div class="collapse show fade active" :id="`return_flight_modal_collapse_${index}`"
                   v-if="show_more_return_flight_index === index">
                <div :class="{'row': true}">
                  <div
                      :class="{'col-12 col-md-4 mt-4': true}"
                      v-for="(provider, providerIndex) in flight.providers" :key="providerIndex">
                    <div
                        :class="{'border p-3 rounded': true, 'flight-active': return_flight_index === index && providerIndex === return_flight_provider_index}">
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>{{ provider.provider_name }}</h5>
                        <img :src="provider.provider_logo" :alt="provider.provider_name" width="35" height="35">
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>{{ provider.go_date }}</h5>
                        <h5 class="mt-2">{{ provider.go_time }}</h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>{{ provider?.return_date }}</h5>
                        <h5 class="mt-2">{{ provider?.return_time }}</h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center">
                        <h5>تعداد صندلی</h5>
                        <h5 class="mt-2">{{ provider.seat }}</h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center mt-2">
                        <h5>هر نفر :</h5>
                        <h5>
                          <span>{{ Number(provider.price).toLocaleString() }}</span>
                          <span> تومان </span>
                        </h5>
                      </div>
                      <hr>
                      <div class="d-flex justify-content-between align-items-center mt-2">
                        <h5>قیمت کل :</h5>
                        <h5>
                          <span>{{ Number(provider.price * adults).toLocaleString() }}</span>
                          <span> تومان </span>
                        </h5>
                      </div>
                      <hr>
                      <div class="">
                        <button v-if="return_flight_index === index && providerIndex === return_flight_provider_index"
                                class="btn btn-success w-100">انتخاب شده
                        </button>
                        <button v-on:click="setReturnFlight(index, providerIndex)" class="btn btn-success w-100" v-else>
                          انتخاب
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import BuildComponent from "@/components/BuildTour/BuildComponent";
import BuildTourAnalyse from "@/components/BuildTour/BuildTourAnalyse";

export default {
  name: "BuildResult",
  components: {BuildTourAnalyse, BuildComponent},
  props: {
    data: Object,
    analysis_data: Object,
    analysis_loading: Boolean,
    adults: Number,
    target: String,
    source: String,
   
  },
  data() {
    return {
      go_flight_index: 0,
      go_flight_provider_index: 0,
      show_more_go_flight_index: -1,
      return_flight_index: 0,
      return_flight_provider_index: 0,
      show_more_return_flight_index: -1,

      //============ New code =====
      // مقادیر اولیه
      // static_hotels: [],
      // hotels: [],
      // go_flight: [],
      // return_flight: [],
      //================
      // ----------
      static_hotels: this.data.hotel,
      hotels: this.data.hotel,
      go_flight: this.getGoFlights(),
      return_flight: this.getReturnFlights(),
      
    
      

      // hotel_names: this.data.hotel.map(hotel => hotel.hotel_name),
      // Example hotels array
      hotel_names:
                [
                { name: 'Hotel A', min_price: 100 },
                { name: 'Hotel B', min_price: 50 },
                { name: 'Hotel C', min_price: undefined }, // Example of missing min_price
                ],
      // ----------
      show_analysis: false,
      // ----------
      last_search: {
        date: new Date(),
        minute: 0,
        text: "ثانیه",
        interval: null,
        warning: false
      },
      // ----------
      filter: {
        hotel: []
      }
    }
  },


  // //============== new Code ===========
  // mounted() {
  //   // بعد از رندر شدن کامپوننت و وقتی داده‌ها آماده شد، این متدها فراخوانی می‌شوند
  //   this.static_hotels = this.data.hotel;
  //   this.hotels = this.data.hotel;
  //   this.go_flight = this.getGoFlights();
  //   this.return_flight = this.getReturnFlights();
    

  // },
  //=====================



  methods: {
    getGoFlights() {
      // let result = [];
      // for (let flight of this.data.flight.go_flight) {
      //   const providers = flight.providers.filter(flg => flg.seat >= this.adults);
      //  if (providers.length) {
      //    flight.providers = providers;
      //   result.push(flight);
      //  }
      // }
      try {
      let result = [];
      console.log("===============================================")
      console.log(this.data.flight)
      console.log("===============================================")
      for (let flight of this.data.flight.go_flight) {
        const providers = flight.providers.filter(flg => flg.seat >= this.adults);
        if (providers.length) {
          flight.providers = providers;
          result.push(flight);
        }
      }
      
      return result; // Return the filtered flights

    } catch (error) {
      console.error("An error occurred while getting go flights:", error);
      //this.showError("خطایی در دریافت اطلاعات پروازها رخ داده است. لطفا مجددا تلاش کنید.");
      return []; // Return an empty array or handle the error appropriately
    }


  
      // return this.data.flight.go_flight;
      




    },
    getReturnFlights() {
      // let result = [];
      // for (let flight of this.data.flight.return_flight) {
      //   const providers = flight.providers.filter(flg => flg.seat >= this.adults);
      //   if (providers.length) {
      //     flight.providers = providers;
      //     result.push(flight);
      //   }
      // }


      try {
      let result = [];
      for (let flight of this.data.flight.return_flight) {
        const providers = flight.providers.filter(flg => flg.seat >= this.adults);
        if (providers.length) {
          flight.providers = providers;
          result.push(flight);
        }
      }
      
      return result; // Return the filtered flights

    } catch (error) {
      console.error("An error occurred while getting return flights:", error);
      //this.showError("خطایی در دریافت اطلاعات پروازها رخ داده است. لطفا مجددا تلاش کنید.");
      return []; // Return an empty array or handle the error appropriately
    }



      // return this.data.flight.return_flight;
    },
    setGoFlight(index, provider_index) {
      this.go_flight_index = index;
      this.go_flight_provider_index = provider_index;
    },
    setReturnFlight(index, provider_index) {
      this.return_flight_index = index;
      this.return_flight_provider_index = provider_index;
    },
    showMoreGo(index) {
      this.show_more_go_flight_index = this.show_more_go_flight_index === index ? -1 : index;
    },
    showMoreReturn(index) {
      this.show_more_return_flight_index = this.show_more_return_flight_index === index ? -1 : index;
    },
    calc_last_search() {
      clearInterval(this.last_search.interval);
      this.last_search.date = new Date();
      this.last_search.minute = 0;
      this.last_search.text = "ثانیه";
      this.last_search.interval = setInterval(() => {
        let new_date = new Date();
        let dif = new_date - this.last_search.date;
        let result = Math.abs(dif / 1000);
        if (result < 60) {
          this.last_search.minute = Math.round(result);
          this.last_search.text = "ثانیه";
        } else {
          this.last_search.minute = parseFloat(result / 60).toFixed(1);
          this.last_search.text = "دقیقه";
        }
        this.last_search.warning = result > 75;
      }, 1000);
    },
    calcGoFlightDif(price) {
      const current_price = this.go_flight[this.go_flight_index].providers[this.go_flight_provider_index].price;
      return price - current_price;
    },
    calcReturnFlightDif(price){
      const current_price = this.return_flight[this.return_flight_index].providers[this.return_flight_provider_index].price;
      return price - current_price;
    },
    closeAnalysis(val) {
      return this.show_analysis = val;
    }
  },
  created() {

    // hazf kardim
    this.hotels = this.hotels.sort((a, b) => a.min_price - b.min_price);
    //=============

    console.log("hotel length ==="+self.hotels);

    // console.log(this.hotels);
    // ---- go flight
    let go_flight_result = [];
    for (let go_flight_item of this.go_flight) {
      const go_providers = go_flight_item.providers.filter(pv => pv.seat >= this.adults);
      if (go_providers.length) {
        go_flight_item.providers = go_providers;
        go_flight_result.push(go_flight_item);
      }
    }
    this.go_flight = go_flight_result;
    // ---- return flight
    let return_flight_result = [];
    for (let return_flight_item of this.return_flight) {
      const return_providers = return_flight_item.providers.filter(pv => pv.seat >= this.adults);
      if (return_providers.length) {
        return_flight_item.providers = return_providers;
        return_flight_result.push(return_flight_item);
      }
    }
    this.return_flight = return_flight_result;
    // ----
    this.calc_last_search();
  },
  watch: {
    "filter.hotel": function (val) {
      let result = []
      if (val.length) {
        val.forEach(s_hotel => {
          const fil = this.static_hotels.filter(hotel => hotel.hotel_name === s_hotel);
          if (fil) {
            result.push(fil[0]);
          }
        })
        this.hotels = result;
      } else {
        console.log('empty', this.static_hotels);
        this.hotels = this.static_hotels;
      }
    }
  },
  unmounted() {
    clearInterval(this.last_search.interval);
  }
}
</script>

<style scoped>
.flight-active {
  background: #c0e3ca !important;
  transition: all .5s;
}
</style>