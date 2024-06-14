<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="styles.css">
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.js" defer></script>
  <title>Daily Blogs</title>
  <style>
    body {
      background-color: #f0f0f0;
    }
    .animated-button {
      z-index: 1;
      position: relative;
    }
    .animated-button::after {
      content: '';
      z-index: -1;
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      transform: translateX(-100%);
      transition: transform 600ms cubic-bezier(0, .70, .60, 1);
    }
    .animated-button:hover::after {
      transform: translateX(0);
    }
    .developer-credit {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background-color: rgba(240, 240, 240, 0.9);
      padding: 5px 10px;
      border-radius: 5px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .developer-credit a {
      color: #555;
      font-weight: 600;
      text-decoration: none;
      transition: color 0.2s ease;
    }
    .developer-credit a:hover {
      color: #333;
    }
  </style>
</head>
<body>
  <div class="flex min-h-screen items-center justify-center">
    <div class="mx-auto w-full max-w-lg rounded-lg bg-white px-10 py-8 shadow-xl">
      <div class="mx-auto space-y-6">
        <h2 style="text-align: center; font-size: x-large; font-weight: 500;">
          Bakshish's Daily Blogs
        </h2>

        <div class="w-screen flex items-center justify-center bg-gray-200 " style="max-width: -webkit-fill-available; border-radius: 10px; max-height: 25vh;">
          <div class="antialiased sans-serif">
            <div x-data="app()" x-init="[initDate(), getNoOfDays()]" x-cloak>
              <div class="container mx-auto px-4 py-2 md:py-10">
                <div class="mb-5 w-64">
                  <label for="datepicker" class="font-bold mb-1 text-gray-700 block">Select Date</label>
                  <div class="relative">
                    <input type="hidden" name="date" x-ref="date">
                    <input 
                      type="text"
                      readonly
                      x-model="datepickerValue"
                      @click="showDatepicker = !showDatepicker"
                      @keydown.escape="showDatepicker = false"
                      class="w-full pl-4 pr-10 py-3 leading-none rounded-lg shadow-sm focus:outline-none focus:shadow-outline text-gray-600 font-medium"
                      placeholder="Select date">
    
                    <div class="absolute top-0 right-0 px-3 py-2">
                      <svg class="h-6 w-6 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                      </svg>
                    </div>
    
                    <div 
                      class="bg-white mt-12 rounded-lg shadow p-4 absolute top-0 left-0" 
                      style="width: 16rem; z-index: 1;" 
                      x-show.transition="showDatepicker"
                      @click.away="showDatepicker = false">
    
                      <div class="flex justify-between items-center mb-2">
                        <div>
                          <span x-text="MONTH_NAMES[month]" class="text-lg font-bold text-gray-800"></span>
                          <span x-text="year" class="ml-1 text-lg text-gray-600 font-normal"></span>
                        </div>
                        <div>
                          <button 
                            type="button"
                            class="transition ease-in-out duration-100 inline-flex cursor-pointer hover:bg-gray-200 p-1 rounded-full" 
                            :class="{'cursor-not-allowed opacity-25': month == 0 }"
                            :disabled="month == 0 ? true : false"
                            @click="month--; getNoOfDays()">
                            <svg class="h-6 w-6 text-gray-500 inline-flex" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/>
                            </svg>  
                          </button>
                          <button 
                            type="button"
                            class="transition ease-in-out duration-100 inline-flex cursor-pointer hover:bg-gray-200 p-1 rounded-full" 
                            :class="{'cursor-not-allowed opacity-25': month == 11 }"
                            :disabled="month == 11 ? true : false"
                            @click="month++; getNoOfDays()">
                            <svg class="h-6 w-6 text-gray-500 inline-flex" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
                            </svg>									  
                          </button>
                        </div>
                      </div>
    
                      <div class="flex flex-wrap mb-3 -mx-1">
                        <template x-for="(day, index) in DAYS" :key="index">	
                          <div style="width: 14.26%" class="px-1">
                            <div
                              x-text="day" 
                              class="text-gray-800 font-medium text-center text-xs"></div>
                          </div>
                        </template>
                      </div>
    
                      <div class="flex flex-wrap -mx-1">
                        <template x-for="blankday in blankdays">
                          <div 
                            style="width: 14.28%"
                            class="text-center border p-1 border-transparent text-sm"	
                          ></div>
                        </template>	
                        <template x-for="(date, dateIndex) in no_of_days" :key="dateIndex">	
                          <div style="width: 14.28%" class="px-1 mb-1">
                            <div
                              @click="getDateValue(date)"
                              x-text="date"
                              class="cursor-pointer text-center text-sm leading-none rounded-full leading-loose transition ease-in-out duration-100"
                              :class="{'bg-blue-500 text-white': isToday(date) == true, 'text-gray-700 hover:bg-blue-200': isToday(date) == false }"	
                            ></div>
                          </div>
                        </template>
                      </div>
                    </div>
                  </div>	 
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <a id="selected-date-link" class="block w-full transform rounded-md bg-teal-400 px-4 py-2 text-center font-medium tracking-wide text-white transition-colors duration-300 hover:bg-teal-500 focus:outline-none focus:ring focus:ring-teal-300 focus:ring-opacity-80"> Go to Selected Date </a>
        <div id="message" class="text-red-500 text-center mt-2 hidden">Invalid date or blog not available</div>
    
        <div class="flex w-full justify-center">
          <a href='showall.html'> 
            <button class="group relative overflow-hidden rounded bg-sky-400 px-2 py-1 text-center font-medium shadow ring-sky-500 transition-all after:bg-sky-500 active:shadow-md active:ring-2 animated-button" style="z-index: 0;">
              <p class="text-white transition-all group-active:scale-90" style="padding: 0.188rem;">  Show All Blogs  </p>
            </button>
          </a>
        </div>
      </div>
    </div>
  </div>

  <div class="developer-credit">
    Developed by <a href="https://github.com/bakshish007" target="_blank" style="color: #555; font-weight: 600;">@Bakshish Singh</a>
  </div>

  <script>
    const MONTH_NAMES = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
    const DAYS = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

    function app() {
      return {
        showDatepicker: false,
        datepickerValue: '',
        month: '',
        year: '',
        no_of_days: [],
        blankdays: [],
        days: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],

        initDate() {
          let today = new Date();
          this.month = today.getMonth();
          this.year = today.getFullYear();
          this.datepickerValue = new Date(this.year, this.month, today.getDate()).toDateString();
        },

        isToday(date) {
          const today = new Date();
          const d = new Date(this.year, this.month, date);
          return today.toDateString() === d.toDateString();
        },

        getDateValue(date) {
          let selectedDate = new Date(this.year, this.month, date);
          this.datepickerValue = selectedDate.toDateString();
          this.$refs.date.value = selectedDate.getFullYear() + "-" + ('0' + (selectedDate.getMonth() + 1)).slice(-2) + "-" + ('0' + selectedDate.getDate()).slice(-2);
          this.showDatepicker = false;

          const formattedDate = `${('0' + selectedDate.getDate()).slice(-2)}-${('0' + (selectedDate.getMonth() + 1)).slice(-2)}-${selectedDate.getFullYear()}`;
          const blogUrl = `blogs/${formattedDate}.html`;

          fetch(blogUrl)
            .then(response => {
              if (response.ok) {
                document.getElementById("selected-date-link").href = blogUrl;
                document.getElementById("message").classList.add("hidden");
              } else {
                document.getElementById("selected-date-link").removeAttribute("href");
                document.getElementById("message").classList.remove("hidden");
              }
            })
            .catch(error => {
              document.getElementById("selected-date-link").removeAttribute("href");
              document.getElementById("message").classList.remove("hidden");
            });
        },

        getNoOfDays() {
          let daysInMonth = new Date(this.year, this.month + 1, 0).getDate();

          // find where to start calendar day of week
          let dayOfWeek = new Date(this.year, this.month).getDay();
          let blankdaysArray = [];
          for (var i = 1; i <= dayOfWeek; i++) {
            blankdaysArray.push(i);
          }

          let daysArray = [];
          for (var i = 1; i <= daysInMonth; i++) {
            daysArray.push(i);
          }

          this.blankdays = blankdaysArray;
          this.no_of_days = daysArray;
        }
      }
    }
  </script>
</body>
</html>
