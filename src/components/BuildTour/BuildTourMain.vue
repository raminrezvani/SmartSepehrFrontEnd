<template>
  <main class="container my-3">
    <!-- --- SEARCH --- -->
    <div class="row align-items-end">
      <!-- --- SOURCE --- -->
      <div class="col-12 col-xl-4">
        <div class="d-flex justify-content-between align-items-center">
          <label for="build_tour_source" class="text-muted">مبدا</label>
          <button class="btn btn-sm" v-if="body.target === 'GSM'" data-bs-toggle="modal"
                  data-bs-target="#alertTargetModal">
            <svg width="16" height="16"
                 fill="currentColor"
                 class="bi bi-exclamation-circle text-danger me-1" viewBox="0 0 16 16">
              <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
              <path
                  d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 4.995z"/>
            </svg>
          </button>
        </div>
        <select name="build_tour_source" id="build_tour_source" class="form-select form-select-sm mt-2"
                v-model="body.source" v-on:change="getCalendarData">
          <option value="MHD">مشهد</option>
          <option value="THR">تهران</option>
          <option value="IFN">اصفهان</option>
          <option value="SYZ">شیراز</option>
          <option value="TBZ">تبریز</option>
          <option value="GSM">قشم</option>
          <option value="KIH">کیش</option>
          <option value="AZD">یزد</option>
          <option value="AWZ">اهواز</option>
          <option value="BND">بندرعباس</option>
          <option value="KER">کرمان</option>
          <option value="KSH">کرمانشاه</option>
          <option value="RAS">رشت</option>
          <option value="SRY">ساری</option>
          <option value="ZBR">چابهار</option>
          <!-- <option value="DXB">دبی</option>
          <option value="IST">استانبول</option>   -->
   
          <option value="ABD">آبادان</option>
          <option value="BUZ">بوشهر</option>
          <option value="GBT">گرگان</option>
          <option value="OMH">ارومیه</option>
          <option value="ADU">اردبیل</option>
          <option value="HDM">همدان</option>
          <option value="RZR">رامسر</option>
          <option value="KHD">خرم آباد</option>
          <!-- <option value="NSH">نوشهر</option> -->
          



        </select>
      </div>


      

      <!-- --- Target --- -->
      <div class="col-12 col-xl-4">
        <div class="d-flex justify-content-between align-items-center">
          <label for="build_tour_target" class="text-muted">مقصد</label>
          <button class="btn btn-sm" v-if="body.target === 'GSM'" data-bs-toggle="modal"
                  data-bs-target="#alertTargetModal">
            <svg width="16" height="16"
                 fill="currentColor"
                 class="bi bi-exclamation-circle text-danger me-1" viewBox="0 0 16 16">
              <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
              <path
                  d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 4.995z"/>
            </svg>
          </button>
        </div>
        <select name="build_tour_target" id="build_tour_target" class="form-select form-select-sm mt-2"
                v-model="body.target" v-on:change="getCalendarData">
          <!-- <option value="KIH">کیش</option>
          <option value="GSM">قشم</option>
          <option value="THR">تهران</option>
          <option value="IFN">اصفهان</option>
          <option value="SYZ">شیراز</option>
          <option value="TBZ">تبریز</option>
          <option value="MHD">مشهد</option> 
          <option value="DXB">دبی</option>
          <option value="IST">استانبول</option> -->

          <option value="MHD">مشهد</option>
          <option value="THR">تهران</option>
          <option value="IFN">اصفهان</option>
          <option value="SYZ">شیراز</option>
          <option value="TBZ">تبریز</option>
          <option value="GSM">قشم</option>
          <option value="KIH">کیش</option>
          <option value="AZD">یزد</option>
          <option value="AWZ">اهواز</option>
          <option value="BND">بندرعباس</option>
          <option value="KER">کرمان</option>
          <option value="KSH">کرمانشاه</option>
          <option value="RAS">رشت</option>
          <option value="SRY">ساری</option>
          <option value="ZBR">چابهار</option>
           <!-- <option value="DXB">دبی</option>
          <option value="IST">استانبول</option>   -->

          <option value="ABD">آبادان</option>
          <option value="BUZ">بوشهر</option>
          <option value="GBT">گرگان</option>
          <option value="OMH">ارومیه</option>
          <option value="ADU">اردبیل</option>
          <option value="HDM">همدان</option>
          <option value="RZR">رامسر</option>
          <option value="KHD">خرم آباد</option>
          <!-- <option value="NSH">نوشهر</option> -->


        </select>
      </div>


      

      <!-- --- GO DATE DESKTOP --- -->
      <div class="d-none d-lg-block col-lg-3 mt-3 mt-lg-0">
        <div class="d-flex justify-content-between align-items-center">
          <label for="date" class="text-muted mb-1">تاریخ</label>
        </div>  
        <calender-index
            key="go_date"
            placeholder="تاریخ رفت"
            :show="show_datepicker"
            :disable-old="true"
            :days_data="calendar_data.go"
            @submitted="goDateSubmit"
            @showing="datePickerShowing"
        />
      </div>
      <!-- --- BACK DATE DESKTOP --- -->
      <div class="d-none d-lg-block col-lg-3 mt-3 mt-lg-0">
        <div class="d-flex justify-content-between align-items-center">
          <label for="night_count" class="text-muted">تاریخ برگشت</label>
          <p class="m-0">
            <span>{{ body.night_count }}</span>
            <span class="me-1">شب</span>
          </p>
        </div>
        <calender-index
            key="back_date"
            placeholder="تاریخ برگشت"
            :show="show_datepicker"
            :disable-old="true"
            :days_data="calendar_data.return"
            @submitted="returnDateSubmit"
            @showing="datePickerShowing"
        />
      </div>
      <!-- --- GO DATE MOBILE --- -->
      <div class="col-12 d-lg-none mt-3 mt-lg-0">
        <div class="d-flex justify-content-between align-items-center">
          <label for="date" class="text-muted mb-1">تاریخ</label>
        </div>
        <calender-index
            key="go_date"
            placeholder="تاریخ رفت"
            :show="show_datepicker_go"
            :disable-old="true"
            :days_data="calendar_data.go"
            @submitted="goDateSubmit"
            @showing="datePickerShowing"
        />
      </div>
      <!-- --- BACK DATE MOBILE --- -->
      <div class="col-12 d-lg-none mt-3 mt-lg-0">
        <div class="d-flex justify-content-between align-items-center">
          <label for="night_count" class="text-muted">تاریخ برگشت</label>
          <p class="m-0">
            <span>{{ body.night_count }}</span>
            <span class="me-1">شب</span>
          </p>
        </div>
        <calender-index
            key="back_date"
            placeholder="تاریخ برگشت"
            :show="show_datepicker_return"
            :disable-old="true"
            :days_data="calendar_data.return"
            @submitted="returnDateSubmit"
            @showing="datePickerShowing"
        />
      </div>
      <!-- --- MODAL --- -->
      <div class="modal fade" id="alertTargetModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="exampleModalLabel">ملاحظات</h1>
            </div>
            <div class="modal-body" style="line-height: 2">
              <p class="m-0 lh-2">برای تور دست ساز قشم٬ فقط روز های دوشنبه٬ سه شنبه٬ پنج شنبه و جمعه مجاز به جستجو
                میباشند.</p>
            </div>
          </div>
        </div>
      </div>
      <!-- --- SUBMIT --- -->
      <div class="col-12 col-lg-2 mt-3 mt-lg-0">
        <button class="btn btn-sm btn-success w-100" :disabled="loading" v-on:click="getData(true)">جستجو</button>
      </div>

      <!-- --- NO CACHE BUTTON --- -->
      <div class="col-12 mt-3">
        <button class="btn btn-success w-100" v-on:click="getData(false)">بروزرسانی زنده</button>
      </div>




      
      <!-- --- PROVIDER --- -->
      <div class="col-12 mt-3 d-flex align-items-center justify-content-between">
        <div class="position-relative w-100 border rounded mt-4 p-2 bg-white">
          <div class="cursor-pointer h-100 w-100" v-on:click="show_provider = !show_provider">
            <p class="m-0">تامین کنندگان</p>
          </div>
          <div class="provider-overlay" v-if="show_provider" v-on:click="show_provider = false"></div>
          <div class="position-absolute w-100 p-3 provider-item bg-white rounded border" v-if="show_provider">
            <div class="d-flex justify-content-between align-items-center position-relative">
              <div>
                <input type="checkbox" id="filter_provider_all" class="form-check-input" :value="true"
                        v-model="allProviderFilter" v-on:change="allProviderFilterMethod">
                <label for="filter_provider_all" class="form-check-label me-1">all</label>
              </div>
              <div class="m-0 d-flex justify-content-between align-items-center tooltip-custom-main">
                {{ allProviderLength }}
              </div>
            </div>
            <div class="d-flex justify-content-between align-items-center mt-3 position-relative"
                  v-for="(provider, index) of providerLength" :key="index">
              <div>
                <input type="checkbox" :id="`filter_provider_${provider.name}`" class="form-check-input"
                        :value="true" :data-value="filter_provider[provider.name]"
                        v-model="filter_provider[provider.name]" v-on:change="sortedData">
                <label :for="`filter_provider_${provider.name}`" class="form-check-label me-1">
                  <a :href="provider.url" target="_blank" class="m-0 link-dark">{{ provider.name }}</a>
                </label>
              </div>
              <div class="m-0 d-flex justify-content-between align-items-center tooltip-custom-main">
                <p>{{ provider.count }}</p>
                <svg v-if="provider.count === 0" xmlns="http://www.w3.org/2000/svg" width="16" height="16"
                      fill="currentColor"
                      class="bi bi-exclamation-circle text-danger me-1" viewBox="0 0 16 16">
                  <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                  <path
                      d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 4.995z"/>
                </svg>
                <div v-if="provider.count === 0" class="position-absolute p-2 tooltip-custom">
                  {{ provider.message }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

            <!-- --- STAR --- -->
      <div class="col-12">
        <label for="filter_star" class="text-muted mb-1">ستاره هتل</label>
        <select v-model="filter_star" class="form-select" id="filter_star" @change="sortedData">
          <option value="all">همه</option>
          <option value="1">۱ ستاره</option>
          <option value="2">۲ ستاره</option>
          <option value="3">۳ ستاره</option>
          <option value="4">۴ ستاره</option>
          <option value="5">۵ ستاره</option>
        </select>
      </div>


    </div>
    <hr>



    <!-- --- MAIN --- -->
    <section>
      <build-loading v-if="loading"></build-loading>
      <build-result v-if="show_result"    :key="dataKey"       :data="data" :adults="parseInt(this.body.adults)"
                    :target="body.target"  :source="body.source" :analysis_data="analyse_data" :analysis_loading="show_analyse_loading"></build-result>
    </section>
  </main>
</template>

<script>
import moment from "moment";
import moment_jalali from "moment-jalaali";
import BuildLoading from "@/components/BuildTour/BuildLoading";
import BuildResult from "@/components/BuildTour/BuildResult";
import {toast} from "vue3-toastify";
import CalenderIndex from "@/components/DatePicker/CalendarIndex";
import {router} from "@/routes";

export default {
  name: "BuildTourMain",
  components: {CalenderIndex, BuildResult, BuildLoading},
  data() {
    return {
      dataKey: 0, // Initialize a key for the component
      show_provider: false,
      filter_provider: {

        Eghamat24: true,
        Expedia:true,
        Alaedin: true,
        alibaba: true,
        booking: true,
        Jimboo: true,
        Snapp:true,
        Stayforlong:true,
        "ZenHotels.com":true,
        "Trip.com":true,
        "Hotels.com":true,
        "Booking.com":true,


        alwin: true,
        deltaban: true,
        sepid_parvaz: true,
        dayan: true,
        omid_oj: true,
        parmis: true,
        mehrab: true,
        rahbal: true,
        kimiya:true,
        eram2mhd:true,
        tak_setareh: true,
        hrc: true,
        hamood:true,
        safiran:true,
        FlyToday:true,
        darvishi:true,
        moeindarbari:true


      },
      provider_length: {},
      filter_star: "all",
      filter_name: "",
      body: {
        "start_date": "",
        "end_date": "",
        "night_count": 0,
        "hotel_star": 5,
        "source": "MHD",
        "target": "KIH",
        "adults": 2,
        "use_cache":true
      },
      calendar_data: {
        go: [],
        return: [],
      },
      show_datepicker: false,
      show_datepicker_go: false,
      show_datepicker_return: false,
      data: {},
      analyse_data: [],
      show_analyse_loading: false,
      loading: false,
      fixed_data: [],
      show_result: false,
      datepicker_min_date: "",
      minimum_date: ""
    }
  },
  methods: {
    getSortedData(data) {

      return data;
      // switch (this.order_by) {
      //   case "price_a": {
      //     return data.hotel.sort((a, b) => parseFloat(a.min_price) - parseFloat(b.min_price));
      //   }
      //   case "price_d": {
      //     return data.hotel.sort((a, b) => parseFloat(b.min_price) - parseFloat(a.min_price));
      //   }
      //   // case "hour_d_a": {
      //   //   return data.hotel.sort((a, b) => parseInt(a.providers[0].go_flight_arrive_time.slice(0, 3)) - parseInt(b.providers[0].go_flight_arrive_time.slice(0, 3)));
      //   // }
      //   // case "hour_d_d": {
      //   //   return data.hotel.sort((a, b) => parseInt(b.providers[0].go_flight_arrive_time.slice(0, 3)) - parseInt(a.providers[0].go_flight_arrive_time.slice(0, 3)));
      //   // }
      //   // case "hour_a_a": {
      //   //   return data.hotel.sort((a, b) => parseInt(a.providers[0].return_flight_arrive_time.slice(0, 3)) - parseInt(b.providers[0].return_flight_arrive_time.slice(0, 3)));
      //   // }
      //   // case "hour_a_d": {
      //   //   return data.hotel.sort((a, b) => parseInt(b.providers[0].return_flight_arrive_time.slice(0, 3)) - parseInt(a.providers[0].return_flight_arrive_time.slice(0, 3)));
      //   // }
      //   default: {
      //     return data.hotel;
      //   }
      // }
    },
    sortedData() {
      const data = this.getSortedData(this.filterProvider());
      if (this.filter_name.length) {
        let valid_filter_name = Object.values(this.filter_name);
        const result = data.hotel.filter(item => valid_filter_name.includes(item.hotel_name));
        this.data.hotel = result;
        this.dataKey++;
        console.log( this.data)
        return result;
      } else {
        if (this.filter_star === "all") {
          const result = data;
          this.data = result;
          this.dataKey++;
          return result;
        } else {
          console.log('Filter Star');

          const result = data.hotel.filter(item => String(item.hotel_star) === String(this.filter_star));
          this.data.hotel = result;
          this.dataKey++;
          return this.data;
        }
      }
    },


    // filterProvider() {
    //   let hotel_provider = [];
    //   this.dataKey++; // Increment the key to trigger re-render



    //   // Make a deep copy of fixed_data to prevent modification of original data
    //   const fixedDataCopy = JSON.parse(JSON.stringify(this.fixed_data));


    //   // console.log(fixedDataCopy.hotel);
    //   console.log(this.filter_provider)



    //   for (let hotel of fixedDataCopy.hotel) {
    //     let hotel_item = {...hotel};
    //     let providers = [];



    //   // hotel_item.providers.forEach(provider => {

    //     if (this.filter_provider.Eghamat24 && hotel.provider === "Eghamat24") {
    //       providers.push({...hotel});
    //     }

    //     if (this.filter_provider.alibaba && hotel.provider === "alibaba") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.booking && hotel.provider === "booking") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.alwin && hotel.provider === "alwin") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.deltaban && hotel.provider === "deltaban") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.sepid_parvaz && hotel.providere === "sepid_parvaz") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.dayan && hotel.provider === "dayan") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.mehrab && hotel.provider=== "mehrab") {
    //       providers.push({...hotel});
    //     }

    //     //kimiya
    //     if (this.filter_provider.kimiya && hotel.provider === "kimiya") {
    //       providers.push({...hotel});
    //     }

    //     //eram2mhd
    //     if (this.filter_provider.eram2mhd && hotel.provider === "eram2mhd") {
    //       providers.push({...hotel});
    //     }
    //     //safiran
    //     if (this.filter_provider.safiran && hotel.provider === "safiran") {
    //       providers.push({...hotel});
    //     }
    //     //safiran
    //     if (this.filter_provider.hamood && hotel.provider === "hamood") {
    //       providers.push({...hotel});
    //     }

    //     //---------



    //     if (this.filter_provider.rahbal && hotel.provider === "rahbal") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.tak_setare && hotel.provider === "tak_setare") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.hrc && hotel.provider === "hrc") {
    //       providers.push({...hotel});
    //     }



    //     if (this.filter_provider.omid_oj && hotel.provider === "omid_oj") {
    //       providers.push({...hotel});
    //     }
    //     if (this.filter_provider.parmis && hotel.provider === "parmis") {
    //       providers.push({...hotel});
    //     }

    //     //
    //     if (providers.length) {
    //       hotel_item.providers = providers;
    //       hotel_provider.push(hotel_item);
    //     }
    //   }
    //   this.data.hotel = hotel_provider;
    //   // Use Vue's set method to update nested properties so that Vue's reactivity system can track the changes:

    //   // this.data = { ...this.data, hotel: hotel_provider };

    //   this.dataKey++; // Increment the key to trigger re-render
    //   // this.data ={}

    //   console.log(this.data);

    //   return this.data;
    // },

    // -- Created By CHATGPT -----------
    filterProvider() {
        let hotel_provider = [];
        this.dataKey++; // Increment the key to trigger re-render

        // Make a deep copy of fixed_data to prevent modification of original data
        const fixedDataCopy = JSON.parse(JSON.stringify(this.fixed_data));

        console.log(this.filter_provider);

        for (let hotel of fixedDataCopy.hotel) {
          const roomProvider = []; // Array to hold filtered rooms
          // Destructure to exclude rooms
          const { rooms, ...hotelWithoutRooms } = hotel;

          // Filter rooms based on provider
          for (let room of rooms) {
            if (
              (this.filter_provider.Eghamat24 && room.provider === "Eghamat24") ||
              (this.filter_provider.Expedia && room.provider === "Expedia") ||

              (this.filter_provider.alibaba && room.provider === "alibaba") ||
              (this.filter_provider.booking && room.provider === "booking") ||
              (this.filter_provider.Jimboo && room.provider === "Jimboo") ||


              (this.filter_provider.Snapp && room.provider === "Snapp") ||
              


              (this.filter_provider.Stayforlong && room.provider === "Stayforlong") ||
              (this.filter_provider["ZenHotels.com"] && room.provider === "ZenHotels.com") ||
              (this.filter_provider["Trip.com"] && room.provider === "Trip.com") ||
              (this.filter_provider["Hotels.com"] && room.provider === "Hotels.com") ||
              (this.filter_provider["Booking.com"] && room.provider === "Booking.com") ||




            


              (this.filter_provider.Alaedin && room.provider === "Alaedin") ||
              (this.filter_provider.alwin && room.provider === "alwin") ||
              (this.filter_provider.deltaban && room.provider === "deltaban") ||
              (this.filter_provider.sepid_parvaz && room.provider === "sepid_parvaz") ||
              (this.filter_provider.dayan && room.provider === "dayan") ||
              (this.filter_provider.mehrab && room.provider === "mehrab") ||
              (this.filter_provider.kimiya && room.provider === "kimiya") ||
              (this.filter_provider.eram2mhd && room.provider === "eram2mhd") ||
              (this.filter_provider.safiran && room.provider === "safiran") ||
              (this.filter_provider.hamood && room.provider === "hamood") ||
              (this.filter_provider.rahbal && room.provider === "rahbal") ||
              (this.filter_provider.tak_setareh && room.provider === "tak_setareh") ||
              (this.filter_provider.FlyToday && room.provider === "FlyToday") ||
              (this.filter_provider.hrc && room.provider === "hrc") ||
              (this.filter_provider.omid_oj && room.provider === "omid_oj") ||
              (this.filter_provider.darvishi && room.provider === "darvishi") ||
              (this.filter_provider.moeindarbari && room.provider === "moeindarbari") ||

              (this.filter_provider.parmis && room.provider === "parmis")
            ) {
              roomProvider.push(room); // Add room to the filtered list
            }
          }

          // Create hotel_item without rooms and assign filtered rooms
          if (roomProvider.length) {
            const hotel_item = {
              ...hotelWithoutRooms,
              rooms: roomProvider // Assign only the filtered rooms to the hotel
            };
            hotel_provider.push(hotel_item); // Add to hotel provider list
          }
        }

        this.data.hotel = hotel_provider; // Update data with the new hotel list
        //-----------------------
        // Increment the key to trigger re-render 
        this.dataKey++; // Increment the key to trigger re-render
        //-----------------------

        console.log(this.data);

        return this.data;
      },

    allProviderFilterMethod() {
      if (this.allProviderFilter) {
        for (let pv in this.filter_provider) {
          this.filter_provider[pv] = false;
        }
      } else {
        for (let pv in this.filter_provider) {
          this.filter_provider[pv] = true;
        }
      }
      this.filterProvider();

      
    },

    datePickerShowing(val) {
      if (val) {
        this.show_datepicker = val;
      } else {
        if (this.body.start_date.length > 2 && this.body.end_date.length > 2) {
          this.show_datepicker = false;
        }
      }
    },
    getCalendarData() {
      const body = {
        "source": this.body.source,
        "target": this.body.target,
        "skip_month": 0
      };
      this.calendar_data = {go: [], return: []}
      this.$http.post('/get-calendar/', body).then(r => {
        this.calendar_data = r.data;
      }).catch((e) => {
        if (e.response.status === 401) {
          return router.push('/login');
        }
      })
    },
    nextDay() {
      const today = moment(this.body.start_date, "YYYY-MM-DD");
      this.body.start_date = moment(today).add(1, 'd').format("YYYY-MM-DD")
    },
    prevDay() {
      const today = moment(this.body.start_date, "YYYY-MM-DD");
      this.body.start_date = moment(today).subtract(1, 'd').format("YYYY-MM-DD")
    },
    convertDate(date) {
      return moment_jalali(date).format("jYYYY/jMM/jDD");
    },
    getData(use_cache = true) {
      // const gsm_valid_days = [1, 2, 4, 5];
      // if (this.body.target === "GSM" && !gsm_valid_days.includes(moment(this.body.start_date).day())) {
      //   toast.error("لطفا تاریخ معتبر وارد کنید", {
      //     autoClose: 6000,
      //     position: "bottom-left",
      //     rtl: false,
      //     closeOnClick: true
      //   });
      //   return false;
      // }
      if (!this.body.start_date || !this.body.end_date) {
        toast.error("لطفا تاریخ هارا وارد کنید", {
          autoClose: 6000,
          position: "bottom-left",
          rtl: false,
          closeOnClick: true
        });
        return false;
      }
      if (this.body.night_count < 0) {
        toast.error("تاریخ برگشت نمیتواند قبل از تاریخ رفت باشد", {
          autoClose: 6000,
          position: "bottom-left",
          rtl: false,
          closeOnClick: true
        });
        return false;
      }
      if (this.body.night_count < 3) {
        toast.error("تور شما نمیتواند کمتر از ۳ شب باشد", {
          autoClose: 6000,
          position: "bottom-left",
          rtl: false,
          closeOnClick: true
        });
        return false;
      }
      this.body.night_count = parseInt(this.body.night_count);
      this.body.use_cache = use_cache;
      this.$store.state.disable_header_link = true;
      this.loading = true;
      this.show_result = false;
      console.log("this.body == "+this.body);
      this.$http.post('/build-tour/', this.body).then(res => {
        console.log(res);

        this.data = res.data;
        this.provider_length = res.data.providers;
        this.filter_name = "";
        // this.fixed_data = this.data;
        // Make a deep copy of `res.data` to keep `fixed_data` constant
        this.fixed_data = JSON.parse(JSON.stringify(res.data));

        this.show_result = true;
        // this.hotels_name = this.data.sort((a, b) => a.hotel_star - b.hotel_star).map(hotel => hotel.hotel_name);
        this.sortedData();
        // this.getAnalysisData();


        this.getAnalyseData(use_cache);
      }).catch((e) => {
        if (e.response.status === 401) {
          return router.push('/login');
        }
        else if (e.response.status === 504) {
          // Handle Gateway Timeout error
          console.log("Error 504: Gateway Timeout. The server took too long to respond.");
          this.data = { message: "The server took too long to respond. Please try again later." }; 
        }
      }).finally(() => {
        this.$store.state.disable_header_link = false;
        this.loading = false;
        this.show_result = true;
      })
    },
    getAnalyseData(use_cache) {
      this.body.night_count = parseInt(this.body.night_count);
      this.show_analyse_loading = true;
      this.$store.state.disable_header_link = true;
      this.body.range = 7;
      this.body.use_cache = use_cache;
      console.log("this.body == "+this.body);
      this.$http.post('/build-tour-analyse/', this.body, { timeout: 600000000 }).then(res => {
        this.analyse_data = res.data;
      }).catch((e) => {
        if (e.response.status === 401) {
          return router.push('/login');
        }
        else if (e.response.status === 504) {
          // Handle Gateway Timeout error
          this.analyse_data = { message: "The server took too long to respond. Please try again later." };
        }
      }).finally(() => {
      this.$store.state.disable_header_link = false;
        this.show_analyse_loading = false;
      })
    },
    calcNighCount() {
      if (!this.body.start_date || !this.body.end_date) {
        this.body.night_count = 0;
        return false;
      }
      let date_1 = new Date(this.body.start_date);
      let date_2 = new Date(this.body.end_date);
      let difference = date_1.getTime() - date_2.getTime();
      let total_days = Math.ceil(difference / (1000 * 3600 * 24));
      if (total_days < 0) {
        this.body.night_count = Math.abs(total_days);
      } else {
        this.body.night_count = total_days * -1;
      }
      return true
    },
    goDateSubmit(value) {
      const date = value.georgian;
      this.body.start_date = `${date.gy}-${date.gm < 9 ? '0' : ''}${date.gm}-${date.gd < 9 ? '0' : ''}${date.gd}`;
      this.dataKey++;
      this.calcNighCount();
      if (!value.data) {
        toast.warning("در این تاریخ پروازی وجود ندارد.", {
          autoClose: 6000,
          position: "top-left",
          rtl: false,
          closeOnClick: true
        });
      }
    },
    returnDateSubmit(value) {
      const date = value.georgian;
      this.body.end_date = `${date.gy}-${date.gm < 9 ? '0' : ''}${date.gm}-${date.gd < 9 ? '0' : ''}${date.gd}`;
      this.dataKey++;
      this.calcNighCount();
      if (!value.data) {
        toast.warning("در این تاریخ پروازی وجود ندارد.", {
          autoClose: 6000,
          position: "top-left",
          rtl: false,
          closeOnClick: true
        });
      }
    },
  },
  computed: {
    isValidDate() {
      return !moment(this.body.start_date, "YYYY-MM-DD").isAfter(new Date());
    },
    backDate() {
      return moment(this.body.start_date, "YYYY-MM-DD").add(this.body.night_count, 'd').format("YYYY-MM-DD");
    },

    allProviderFilter() {
      for (let pv in this.filter_provider) {
        if (!this.filter_provider[pv]) {
          return false;
        }
      }
      return true
    },

    providerLength() {
      let result = [];
      for (let pv in this.provider_length) {
        result.push({
          "name": pv,
          "count": this.provider_length[pv]['length'],
          "message": this.provider_length[pv]['message'],
          "url": this.provider_length[pv]['url']
        })
      }

      console.log(result);

      return result;
    },
    allProviderLength() {
      let result = 0;
      this.providerLength.forEach(pv => {
        result += pv.count;
      })
      return result
    },
  },
  

  watch: {
    filter_name() {
      this.sortedData();
    },

    filter_provider: {
    handler() {
      this.filterProvider();
    },
    deep: true, // watch for deep changes
  },
  // fixed_data: {
  //   handler() {
  //     this.filterProvider();
  //   },
  //   deep: true,
  // },

  },
  created() {

    // // this.show_analysis = this.$store.state.build_tour_data.show_analysis;
    // // this.loading_analysis = this.$store.state.build_tour_data.loading_analysis;
    // // this.analysis_data = this.$store.state.build_tour_data.analysis_data;
    // // this.order_by = this.$store.state.build_tour_data.order_by;
    // this.show_provider = this.$store.state.build_tour_data.show_provider;
    // this.filter_provider = this.$store.state.build_tour_data.filter_provider;
    // this.provider_length = this.$store.state.build_tour_data.provider_length;
    // // this.hotels_name = this.$store.state.build_tour_data.hotels_name;
    // // this.all_hotels_name = this.$store.state.build_tour_data.all_hotels_name;
    // // this.minimum_date = this.$store.state.build_tour_data.minimum_date;
    // // this.datepicker_min_date = this.$store.state.build_tour_data.datepicker_min_date;
    // this.filter_star = this.$store.state.build_tour_data.filter_star;
    // this.filter_name = this.$store.state.build_tour_data.filter_name;
    // this.body = this.$store.state.build_tour_data.body;
    // this.loading = this.$store.state.build_tour_data.loading;
    // // this.search_start = this.$store.state.build_tour_data.search_start;
    // // this.search_end = this.$store.state.build_tour_data.search_end;
    // // this.search_night_count = this.$store.state.build_tour_data.search_night_count;
    // this.fixed_data = this.$store.state.build_tour_data.fixed_data;
    // this.show_result = this.$store.state.build_tour_data.show_result;
    // // this.show_print = this.$store.state.build_tour_data.show_print;
    // this.data = this.$store.state.build_tour_data.data;
    // this.last_search = this.$store.state.build_tour_data.last_search;
    // this.show_hotel_anaysis = this.$store.state.build_tour_data.show_hotel_anaysis;

    // //
    // function formatDate(date) {
    //   var d = new Date(date),
    //       month = '' + (d.getMonth() + 1),
    //       day = '' + d.getDate(),
    //       year = d.getFullYear();

    //   if (month.length < 2)
    //     month = '0' + month;
    //   if (day.length < 2)
    //     day = '0' + day;

    //   return [year, month, day].join('-');
    // }

    // this.calc_last_search();
    // this.body.start_date = formatDate(new Date());
    // this.datepicker_min_date = formatDate(new Date());
    // const today = moment(this.body.start_date, "YYYY-MM-DD");
    // this.minimum_date = moment(today).subtract(1, 'd').format("YYYY-MM-DD");

    //
    // const data = this.$store.state.build_tour_data;
    // this.body = data.body;
    // this.data = {};
    // this.loading = data.loading;
    // this.show_result = data.show_result;
    // this.datepicker_min_date = data.datepicker_min_date;
    // this.minimum_date = data.minimum_date;
    //
    this.getCalendarData();
  },
  

  beforeUnmount() {
    
    this.$store.state.build_tour_data = this.$data;
  },
  
}
</script>

<style scoped>

</style>