<script>
// Axios for API requests
import axios from 'axios';
export default {
    name: 'home',
    // Theme colour changer button from props
    props: ['isDarkTheme'],
    data() {
        return {
            // API loading animation, error message, API response
            pending: false,
            error: null,
            countryInfo: null,
            // Search bar's default value
            search: '',
            // v-if data "showFilter": toggles hide/display dropdown list
            showFilter: false,
            // v-if data "showAllRegion": After first selection "All Regions" option becomes available
            showAllRegion: false,
            // v-model data "region": Automatically updates the dropdown heading text
            region: '',
            name: '',
            countries: {},
            country: '',
            // Dark/Light theme button toggle
            darkMode: false,
        }
    },
    mounted() {
        // API request using axios to restcountries.eu
        this.pending = true;
        axios
            .get('https://restcountries.eu/rest/v2/all')
            .then(response => (this.countryInfo = response.data))
            .catch(error => (this.error = error))
            .finally(() => { this.pending = false });
    },
    // Function to format the population number with commas
    filters: {
        formatNumbers(value) {
            return `${value.toLocaleString()}`
        }
    },
    // Search and dropdown filter results - Computed function are always watching for changes
    computed: {
        filteredCountries: function() {
            return this.countryInfo.filter((country) => {
                if (this.region === '' || this.region === 'All Regions') {
                    return country.name.toLowerCase().match(this.search.toLowerCase());
                } else if (this.search !== '') {
                    return country.name.toLowerCase().match(this.search.toLowerCase());
                }
            })
        }
    },
    // Vue.JS bug with using v-model and v-on:click with a radio input so need to call this function and run a setTimeout to excute click events after input is checked
    methods: {
        handleFilterClick() {
            setTimeout(() => {
                this.showFilter = !this.showFilter;
                this.showAllRegion = true;
            })
        }
    }
}
</script>
<template>
    <div class="home" :class="{ darkTheme : isDarkTheme }">
        <div class="xd">
            <b-row>
                <b-col md="6">
                    <div class="titlediv">
                        <h1 class="title">COUNTRY SELECTOR</h1>
                    </div>
                    <div class="searchBar">
                        <!-- Top left search bar component -->
                        <div class="dropdownDiv">
                            <a class="dropdownBtn" v-if="!showAllRegion" v-on:click="showFilter = !showFilter">
                           Country
                        </a>
                            <a class="dropdownBtn" v-else v-on:click="showFilter = !showFilter">
                            {{ region }}
                        </a>
                            <!-- Dropdown list of regions -->
                            <ul v-if="showFilter" class="dropdownUL">
                                <li>
                                    <div class="searchContainerdropdown">
                                        <i class="fas fa-search searchIcondropdown"></i>
                                        <input class="searchInput" type="text" v-model="search" aria-label="Search for a country..." placeholder="Search for a country..." />
                                        <ul class="searchResults"></ul>
                                    </div>
                                </li>
                                <li>
                                    <label for="radioAfrica">
                            <input
                              id="radioAmerica"
                              class="dropdownInput"
                              type="radio"
                              v-model="search"
                              v-on:click="handleFilterClick"
                            />
                            </label>
                                    <div v-for="country in filteredCountries" v-bind:key="country.id">
                                        <router-link :to="{ name: 'country-detail', params: {country: country.name}}" class="linkTile lo">
                                            <img v-bind:src="country.flag" alt="Country Flag" class="flag small">
                                            <span class="c-align">&nbsp;&nbsp;&nbsp;{{ country.name }}</span>
                                        </router-link>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </b-col>
                <b-col md="6">
                    <div class="ff">
                        <b-carousel id="carousel-1" :interval="4000" controls indicators background="#ababab" img-width="1024" img-height="480" style="text-shadow: 1px 1px 2px #333;">
                            <b-carousel-slide img-src="https://serving.photos.photobox.com/28434513ae756e14189495b479caf8efa6d757399f4de30cef6195c6497a20916bca9858.jpg" class="fill" height="100"></b-carousel-slide>
                            <b-carousel-slide img-src="https://serving.photos.photobox.com/80520314ecb6771027cf9edd9ae1ce97c731ca8ce8cc08da306edd71e186331f4a8d6836.jpg" class="fill" height="1000">
                            </b-carousel-slide>
                            <b-carousel-slide img-src="https://serving.photos.photobox.com/9974920261b33603902c8173c037082e18cbc101d23369d451f79912e1689fa1e51cfc98.jpg"></b-carousel-slide>
                            <b-carousel-slide>
                                <template v-slot:img>
              <img
                class="d-block img-fluid w-100"
                width="1024"
                height="480"
                src="https://serving.photos.photobox.com/562720658fd0416a914e8dbbffbeba47827235b65a14bc3fd1df76ccbbc6f09f4a236516.jpg"
                alt="image slot"
              >
</template>
      </b-carousel-slide>
    </b-carousel>
    <p class="mt-4">
    </p>
  </div></b-col>
  </b-row>
  </div>
        <h1 v-if="error !== null">Sorry, an error has occurred {{error}}</h1>
        <div class="loaderFlex">
            <div v-if="pending" class="loader"></div>
        </div>
    </div>
</template>
<style scoped>
.titlediv {
    padding: 20% 0px 0px 0px;
    /* margin: 14%; */
}
h1.title {
    /* margin: 44px; */
    color: white;
    font-weight: 600;
}
.ff {
    padding: 35px;
}
.xd {
    background-color: rgba(0, 0, 0, 0.6);
    box-shadow: 1px 1px 7px 0px rgba(255, 255, 255, 1);
}
.home {
    background-image: url(https://images3.alphacoders.com/557/557365.jpg);
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-size: cover;
    height: 100%;
    padding: 10%;
}
.searchBar[data-v-8dc7cce2] {
    padding: 35px 75px;
    display: flex;
    justify-content: space-between;
    max-width: 1400px;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    vertical-align: ce;
}
.searchIcondropdown {
    font-size: 16px;
    color: #848484;
    padding-right: 8px;
}
.searchContainerdropdown {
    border: none;
    border-radius: 5px;
    -webkit-box-shadow: 1px 1px 7px 0px rgb(0, 0, 0, 0.1);
    box-shadow: 1px 1px 7px 0px rgba(0, 0, 0, 0);
    width: 92%;
    height: 50px;
    background-color: #fff;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    padding-left: 7px;
    margin: 3%;
}
ul.dropdownUL {
    overflow-y: auto;
    height: 55%;
}
.linkTile.lo {
    display: inline-block;
    width: 94%;
    height: auto;
    overflow-x: hidden !important;
    text-decoration: none;
    color: inherit;
    margin: 0%;
    padding-left: 6%;
}
img.flag.small {
    height: 10%;
    width: 20%;
    border-radius: 20%;
}
.home {
    background-color: #f9f9f9;
}
.searchBar {
    padding: 35px 75px;
    display: flex;
    justify-content: space-between;
    max-width: 1400px;
    margin: 0 auto;
}
.searchContainer {
    border: none;
    border-radius: 5px;
    box-shadow: 1px 1px 7px 0px rgb(0, 0, 0, 0.1);
    width: 500px;
    height: 50px;
    background-color: #fff;
    display: flex;
    align-items: center;
    padding-left: 30px;
}
.searchIcon {
    font-size: 16px;
    color: #848484;
    padding-right: 30px;
}
.searchInput {
    border: none;
    font-size: 14px;
    font-family: 'Nunito Sans', sans-serif;
    width: 420px;
}
.dropdownBtn {
    background-color: rgba(0, 0, 0, 0.6);
    display: block;
    height: 50px;
    width: 350px;
    box-shadow: 1px 1px 7px 0px rgba(255, 255, 255, 1);
    padding-right: 30px;
    padding-left: 30px;
    border-radius: 3px;
    cursor: pointer;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-content: space-between;
    color: aqua;
}
.dropdownBtn:hover {
    color: white !important;
}
.dropdownBtn::after {
    font-family: "Font Awesome 5 Free";
    font-weight: 900;
    content: "\f107";
}
.dropdownUL {
    background-color: rgba(0, 0, 0, 0.6);
    padding-left: 0;
    text-align: left;
    margin-top: 3px;
    padding: 10px 0;
    border-radius: 3px;
    position: absolute;
    width: 349px;
    box-shadow: 1px 1px 7px 0px rgba(255, 255, 255, 1);
    color: white;
}
li:hover {
    background-color: rgba(0, 0, 0, 0.1) !important;
}
.dropdownUL li {
    list-style: none;
    line-height: 2;
    cursor: pointer;
}
.dropdownUL li label {
    cursor: pointer;
    padding: 0 26px;
    display: block;
    width: 148px;
}
.dropdownUL li:hover {
    background-color: #f9f9f9;
}
.dropdownInput {
    height: 1px;
    clip: rect(1px, 1px, 1px, 1px);
    position: absolute;
}
input[type="radio"] {
    -webkit-appearance: radio;
}
.loader {
    border: 16px solid #f3f3f3;
    border-top: 16px solid #2b3845;
    border-radius: 50%;
    width: 120px;
    height: 120px;
    animation: spin 2s linear infinite;
    margin-bottom: 100px;
}
@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}
.loaderFlex {
    display: flex;
    justify-content: center;
}
.tileGrid {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    padding: 25px 50px 0;
    margin: 0 auto;
    max-width: 1450px;
}
.countryTile {
    display: inline-block;
    background-color: #fff;
    width: 300px;
    box-shadow: 1px 1px 7px 0px rgb(0, 0, 0, 0.1);
    border-radius: 3px;
    cursor: pointer;
    text-align: left;
    margin: 0 25px 80px;
    text-decoration: none;
    color: inherit;
    -webkit-animation: fadein 1s;
    /* Safari, Chrome and Opera > 12.1 */
    -moz-animation: fadein 1s;
    /* Firefox < 16 */
    -ms-animation: fadein 1s;
    /* Internet Explorer */
    -o-animation: fadein 1s;
    /* Opera < 12.1 */
    animation: fadein 1s;
}
@keyframes fadein {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}
/* Firefox < 16 */
@-moz-keyframes fadein {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}
/* Safari, Chrome and Opera > 12.1 */

@-webkit-keyframes fadein {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}
/* Internet Explorer */
@-ms-keyframes fadein {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}
.linkTile {
    display: inline-block;
    width: 300px;
    height: 365px;
    text-decoration: none;
    color: inherit;
}
.flag {
    height: 180px;
    width: 300px;
    border-radius: 3px 3px 0 0;
}
.text {
    padding-left: 30px;
    padding-right: 20px;
}
.text h1 {
    font-size: 20px;
}
.text p span {
    font-weight: 600;
}
.text p {
    line-height: 1;
}
::placeholder {
    font-weight: 600;
}
/* Dark Theme */
.darkTheme,
.darkTheme .dropdownUL li:hover {
    background-color: #202c36;
}
.darkTheme .searchContainer,
.darkTheme .searchInput,
.darkTheme .dropdownBtn,
.darkTheme .dropdownUL,
.darkTheme .countryTile {
    background-color: #2b3845;
}
.darkTheme h1,
.darkTheme p,
.darkTheme .searchIcon,
.darkTheme .searchInput,
.darkTheme ::placeholder,
.darkTheme .dropdownBtn,
.darkTheme .dropdownUL {
    color: #fff;
}
.darkTheme .loader {
    border: 16px solid #2b3845;
    border-top: 16px solid #f3f3f3;
}
@media (max-width: 875px) {
    .tileGrid {
        justify-content: center;
        padding-left: 50px;
    }
    .searchBar {
        flex-direction: column;
        padding: 25px 15px;
    }
    .searchContainer {
        width: inherit;
    }
    .dropdownDiv {
        margin-top: 40px;
    }
}
</style>
