<template>
  <main class="container my-4">
    <!-- --- SEARCH --- -->
    <div class="search-section bg-white rounded-3 p-4 shadow-sm">
      <div class="row g-3">
        <!-- --- SOURCE --- -->
        <div class="col-12 col-md-6 col-lg-2">
          <div class="form-group">
            <label for="build_tour_source" class="text-muted mb-2">مبدا</label>
            <select name="build_tour_source" id="build_tour_source" 
                    class="form-select" 
                    v-model="body.source" 
                    v-on:change="getCalendarData">
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
              <option value="ABD">آبادان</option>
              <option value="BUZ">بوشهر</option>
              <option value="GBT">گرگان</option>
              <option value="OMH">ارومیه</option>
              <option value="ADU">اردبیل</option>
              <option value="HDM">همدان</option>
              <option value="RZR">رامسر</option>
              <option value="KHD">خرم آباد</option>
            </select>
          </div>
        </div>

        <!-- --- Target --- -->
        <div class="col-12 col-md-6 col-lg-2">
          <div class="form-group">
            <label for="build_tour_target" class="text-muted mb-2">مقصد</label>
            <select name="build_tour_target" id="build_tour_target" 
                    class="form-select" 
                    v-model="body.target" 
                    v-on:change="getCalendarData">
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
              <option value="ABD">آبادان</option>
              <option value="BUZ">بوشهر</option>
              <option value="GBT">گرگان</option>
              <option value="OMH">ارومیه</option>
              <option value="ADU">اردبیل</option>
              <option value="HDM">همدان</option>
              <option value="RZR">رامسر</option>
              <option value="KHD">خرم آباد</option>
            </select>
          </div>
        </div>

        <!-- --- GO DATE --- -->
        <div class="col-12 col-md-6 col-lg-2">
          <div class="form-group">
            <label class="text-muted mb-2">تاریخ رفت</label>
            <calender-index
                key="go_date"
                placeholder="تاریخ رفت"
                :show="show_datepicker"
                :disable-old="true"
                :days_data="calendar_data.go"
                @submitted="goDateSubmit"
                @showing="datePickerShowing"
                class="date-picker"
            />
          </div>
        </div>

        <!-- --- BACK DATE --- -->
        <div class="col-12 col-md-6 col-lg-2">
          <div class="form-group">
            <div class="d-flex justify-content-between">
              <label class="text-muted mb-2">تاریخ برگشت</label>
              <span class="text-muted small">{{ body.night_count }} شب</span>
            </div>
            <calender-index
                key="back_date"
                placeholder="تاریخ برگشت"
                :show="show_datepicker"
                :disable-old="true"
                :days_data="calendar_data.return"
                @submitted="returnDateSubmit"
                @showing="datePickerShowing"
                class="date-picker"
            />
          </div>
        </div>

        <!-- --- STAR --- -->
        <div class="col-12 col-md-6 col-lg-2">
          <div class="form-group">
            <label for="filter_star" class="text-muted mb-2">ستاره هتل</label>
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

        <!-- --- PROVIDER --- -->
        <div class="col-12 col-md-6 col-lg-2">
          <div class="form-group">
            <label class="text-muted mb-2">تامین کنندگان</label>
            <div class="position-relative">
              <div class="provider-selector" @click="show_provider = !show_provider">
                <div class="d-flex justify-content-between align-items-center p-2 border rounded">
                  <span>{{ allProviderLength }} تامین کننده</span>
                  <i class="bi" :class="show_provider ? 'bi-chevron-up' : 'bi-chevron-down'"></i>
                </div>
              </div>
              
              <div class="provider-overlay" v-if="show_provider" @click="show_provider = false"></div>
              
              <div class="provider-dropdown" v-if="show_provider">
                <div class="provider-header">
                  <div class="form-check d-flex justify-content-between align-items-center">
                    <div>
                      <input type="checkbox" id="filter_provider_all" class="form-check-input" 
                             :value="true" v-model="allProviderFilter" 
                             @change="allProviderFilterMethod">
                      <label for="filter_provider_all" class="form-check-label">همه</label>
                    </div>
                    <span class="badge bg-primary">{{ allProviderLength }}</span>
                  </div>
                </div>
                
                <div class="provider-list">
                  <div v-for="(provider, index) in providerLength" :key="index" class="provider-item">
                    <div class="d-flex justify-content-between align-items-center p-2">
                      <div class="d-flex align-items-center">
                        <input type="checkbox" 
                               :id="'provider_' + index" 
                               v-model="filter_provider[provider.name]" 
                               :value="true">
                        <label :for="'provider_' + index" class="ms-2">{{ provider.name }}</label>
                      </div>
                      <span :title="provider.message || ''" 
                            :style="{ 
                              cursor: provider.message ? 'help' : 'default',
                              color: provider.message ? '#dc3545' : 'inherit'
                            }">
                        {{ provider.count }}
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- --- BUTTONS --- -->
        <div class="col-12 mt-3">
          <div class="d-flex gap-2">
            <button class="btn btn-primary flex-grow-1" :disabled="loading" @click="getData(true)">
              <i class="bi bi-search me-1"></i>
              جستجو
            </button>
            <button class="btn btn-outline-primary flex-grow-1" @click="getData(false)">
              <i class="bi bi-arrow-clockwise me-1"></i>
              بروزرسانی زنده
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- --- MAIN --- -->
    <section class="mt-4">
      <build-loading v-if="loading"></build-loading>
      <build-result v-if="show_result" 
                   :key="dataKey" 
                   :data="data" 
                   :adults="parseInt(this.body.adults)"
                   :target="body.target" 
                   :body="this.body" 
                   :source="body.source" 
                   :analysis_data="analyse_data" 
                   :analysis_loading="show_analyse_loading">
      </build-result>
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
        moeindarbari:true,

        hamsafar:true,
        iman:true,
        dolfin:true,
        flamingo:true,
        shayan_gasht:true,
        yegane_fard:true,
        alaedin:true,





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



              (this.filter_provider.hamsafar && room.provider === "hamsafar") ||
              (this.filter_provider.iman && room.provider === "iman") ||
              (this.filter_provider.flamingo && room.provider === "flamingo") ||
              (this.filter_provider.shayan_gasht && room.provider === "shayan_gasht") ||
              (this.filter_provider.yegane_fard && room.provider === "yegane_fard") ||
              (this.filter_provider.dolfin && room.provider === "dolfin") ||
              (this.filter_provider.Alaedin && room.provider === "alaedin") ||




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
        this.datakey++;
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
      this.show_analyse_loading = false;
      this.$store.state.disable_header_link = true;
      this.body.range = 7;
      this.body.use_cache = use_cache;
      console.log("this.body == "+this.body);
      this.dataKey++;
      // this.$http.post('/build-tour-analyse/', this.body, { timeout: 600000000 }).then(res => {
      //   this.analyse_data = res.data;
      // }).catch((e) => {
      //   if (e.response.status === 401) {
      //     return router.push('/login');
      //   }
      //   else if (e.response.status === 504) {
      //     // Handle Gateway Timeout error
      //     this.analyse_data = { message: "The server took too long to respond. Please try again later." };
      //   }
      // }).finally(() => {
      // this.$store.state.disable_header_link = false;
      //   this.show_analyse_loading = false;
      // })
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
.search-section {
  border: 1px solid #e9ecef;
}

.form-group {
  margin-bottom: 0;
}

.form-select {
  font-size: 0.9rem;
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #ced4da;
  background-color: #fff;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-image: none !important;
}

.form-select::-ms-expand {
  display: none;
}

.date-picker {
  width: 100%;
}

.provider-selector {
  cursor: pointer;
}

.provider-selector:hover {
  opacity: 0.9;
}

.provider-dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: white;
  border: 1px solid #ced4da;
  border-radius: 4px;
  margin-top: 0.5rem;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  z-index: 1001;
}

.provider-header {
  padding: 1rem;
  border-bottom: 1px solid #e9ecef;
}

.provider-list {
  max-height: 300px;
  overflow-y: auto;
  padding: 0.5rem;
}

.provider-item {
  padding: 0.5rem;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.provider-item:hover {
  background-color: #f8f9fa;
}

.provider-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 999;
}

.btn {
  padding: 0.5rem 1rem;
  font-size: 0.9rem;
  transition: all 0.3s ease;
}

.btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Scrollbar Styling */
.provider-list {
  scrollbar-width: thin;
  scrollbar-color: #888 #f1f1f1;
}

.provider-list::-webkit-scrollbar {
  width: 6px;
}

.provider-list::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 3px;
}

.provider-list::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 3px;
}

.provider-list::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>