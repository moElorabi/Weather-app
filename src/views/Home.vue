<template>
    <div class="home">
        <!-- start navbar -->
        <nav class="navbar navbar-expand-lg fixed-top navbar-dark bg-dark">
            <div class="container-fluid">
                <a class="navbar-brand" href="#"
                    ><img
                        src="https://openweathermap.org/themes/openweathermap/assets/img/logo_white_cropped.png"
                        alt="logo"
                        height="60px"
                /></a>

                <form class="form-inline">
                    <input
                        type="text"
                        class="form-control"
                        placeholder="weather in your city"
                    />
                    <button
                        class="get-button btn btn-outline-success my-2 my-sm-0 "
                        type="button"
                        v-on:click="getWeather"
                    >
                        Search
                    </button>
                </form>

                <div class="dropleft">
                    <b-avatar
                        class="btn btn-lg btn-secondary dropdown-toggle"
                        type="button"
                        id="dropdownMenuButton"
                        data-toggle="dropdown"
                        aria-haspopup="true"
                        aria-expanded="false"
                        rounded="lg"
                        size="3rem"
                    ></b-avatar>
                    <div
                        class="dropdown-menu"
                        aria-labelledby="dropdownMenuButton"
                    >
                        <form class="px-4 py-3">
                            <div class="form-group">
                                <label for="exampleDropdownFormEmail1"
                                    >Email address</label
                                >
                                <input
                                    type="email"
                                    class="form-control"
                                    id="exampleDropdownFormEmail1"
                                    placeholder="email@example.com"
                                />
                            </div>
                            <div class="form-group">
                                <label for="password">Password</label>
                                <input
                                    type="password"
                                    class="form-control"
                                    id="exampleDropdownFormPassword1"
                                    placeholder="Password"
                                />
                            </div>
                            <div class="form-check">
                                <input
                                    type="checkbox"
                                    class="form-check-input"
                                    id="dropdownCheck"
                                />
                                <label
                                    class="form-check-label"
                                    for="dropdownCheck"
                                >
                                    Remember me
                                </label>
                            </div>
                            <button type="button" class="btn btn-primary">
                                Sign in
                            </button>
                        </form>
                        <div class="dropdown-divider"></div>
                        <a class="dropdown-item" href="#"
                            >New around here? Sign up</a
                        >
                        <a class="dropdown-item" href="#">Forgot password?</a>
                    </div>
                </div>
            </div>
        </nav>
        <!-- end navbar -->

        <!-- start weather info container -->
        <div class="container-fluid">
            <div class="row">
                <!-- start weather info div -->
                <div class="show-data col-sm-7">
                    <span
                        >Escreva o nome de qualquer cidade do mundo para saber o
                        clima</span
                    >
                </div>
                <!-- end weather info container -->

                <!-- start weather info history -->
                <div class="col-sm-5 show-data latest-search">
                    <h4>O histórico de pesquisa das cidade</h4>
                </div>
                <!-- end weather info history -->
            </div>
        </div>
        <!-- end weather info container -->
    </div>
</template>

<script>
export default {
    name: "Home",
    methods: {
        getWeather: function() {
            let theInput = document.querySelector(".form-inline input"),
                wData = document.querySelector(".show-data"),
                apiKey = "ed53a1d8a00a52790e432de9d880e752";

            if (theInput.value == "") {
                // If Value Is Empty
                wData.innerHTML =
                    "<span>Escreva o nome de qualquer cidade do mundo para saber o clima</span>";
            } else {
                this.axios
                    .get(
                        `http://api.openweathermap.org/data/2.5/weather?q=${theInput.value}&units=metric&appid=${apiKey}`
                    )
                    .then(data => {
                        // Empty The input after clicking button search
                        theInput.value = "";

                        // Empty The Container
                        wData.innerHTML = "";

                        // Create The Main Div
                        let mainDiv = document.createElement("div");

                        // Create The Div of name and flag
                        let divName = document.createElement("div");

                        // Create The wheather image
                        let wImage = document.createElement("img");
                        wImage.setAttribute(
                            "src",
                            `http://openweathermap.org/img/wn/${data.data.weather[0].icon}@2x.png`
                        );
                        wImage.setAttribute("alt", "wheather image");

                        // Append The image To The Div of name and flag
                        divName.appendChild(wImage);

                        // Create city Name
                        let city = document.createTextNode(
                            `${data.data.name}, ${data.data.sys.country} `
                        );
                        // Append The city Name To The Div of name and flag
                        divName.appendChild(city);

                        // Create The country image
                        let cImage = document.createElement("img");
                        cImage.setAttribute(
                            "src",
                            `http://openweathermap.org/images/flags/${data.data.sys.country.toLowerCase()}.png`
                        );
                        cImage.setAttribute("alt", "card-pic");
                        cImage.style.marginTop = "-5px";

                        // Append The image To Main Div
                        divName.appendChild(cImage);

                        // add style to The Div of name and flag
                        divName.style.color = "#e96e50";
                        divName.style.fontWeight = "bold";

                        // Append The Div of name and flag to the main div
                        mainDiv.appendChild(divName);

                        // Create temp info
                        let tmepSpan = document.createElement("span");
                        let temp = document.createTextNode(
                            `${data.data.main.temp}°С`
                        );

                        // add style to tmepSpan
                        tmepSpan.style.backgroundColor = "#343a40";
                        tmepSpan.style.color = "white";
                        tmepSpan.style.padding = "5px";
                        tmepSpan.style.borderRadius = "10px";
                        tmepSpan.style.fontWeight = "bold";

                        // Append temp info To tmepSpan
                        tmepSpan.appendChild(temp);

                        // Create wheather info
                        let wInfo = document.createTextNode(
                            ` temperatura entre ${data.data.main.temp_min} e ${data.data.main.temp_max} °С, Vento ${data.data.wind.speed} m/s. Nuvens ${data.data.clouds.all} %`
                        );

                        // Append tmepSpan and temp info To Main Div
                        mainDiv.appendChild(tmepSpan);
                        mainDiv.appendChild(wInfo);

                        // Create a line under the div
                        let divider = document.createElement("div");

                        // adding style to the line under the div
                        divider.style.border = "solid 1px #fcbf49";
                        divider.style.width = "auto";
                        divider.style.margin = "auto";
                        divider.style.marginTop = "15px";

                        // append the line under the div to the main div
                        mainDiv.appendChild(divider);

                        // Append The Main Div To Container
                        wData.appendChild(mainDiv);

                        // localStorage for the history
                        if (
                            document.querySelectorAll(".latest-search span")
                                .length < 6
                        ) {
                            // remove webpack from localStorage
                            localStorage.removeItem(
                                "loglevel:webpack-dev-server"
                            );

                            // create localStorage
                            localStorage.history = divName.innerHTML;

                            // showing the history
                            for (let [key, value] of Object.entries(
                                localStorage
                            )) {
                                document.querySelector(
                                    ".latest-search"
                                ).innerHTML += `<span style="color:#e96e50; font-weight:bold; display:block; font-size: 15px;">${(key,
                                value)}<div style = "border-bottom: solid 1px #fcbf49; width: 200px; margin: auto;"></div></span>`;
                            }
                        }
                        // controling the number of the countries in the history div
                        if (
                            document.querySelectorAll(".latest-search span")
                                .length == 6
                        ) {
                            document
                                .querySelector(".latest-search ")
                                .childNodes[1].remove();
                        }
                    })
                    .catch(() => {
                        // Empty The input after clicking button search
                        theInput.value = "";
                        // If Value Is not correct
                        wData.innerHTML =
                            "<span>Escreva o nome correto da cidade</span>";
                    });
            }
        }
    }
};
</script>

<style lang="scss" scoped>
img {
    margin-right: 5px;
}
.form-control:focus {
    box-shadow: none;
}
.form-inline .form-control {
    height: 60px;
    border: none;
}
.form-inline {
    width: 90%;
}
.btn-outline-success {
    height: 60px;
    border: none;
    color: white;
    background-color: peru;
    margin-left: 10px;
    &:hover {
        background-color: #fcbf49;
        border: none;
        color: white;
    }
    &:focus {
        box-shadow: none;
    }
}
.btn-outline-success:not(:disabled):not(.disabled):active:focus {
    box-shadow: none;
}
.btn-outline-success:not(:disabled):not(.disabled):active {
    background-color: #fcbf49;
    border: none;
    color: white;
}
.btn-secondary {
    background-color: peru;
    &:hover {
        background-color: #fcbf49;
        color: white;
    }
}

.btn-secondary:not(:disabled):not(.disabled):active,
.show > .btn-secondary.dropdown-toggle {
    background-color: #fcbf49;
    color: white;
}
.dropdown-menu.show {
    background-color: #003049;
    color: white;
}

.btn-primary {
    background-color: #fcbf49;
    color: white;
    border: none;
    &:hover {
        background-color: #fcbf49;
        color: white;
        border: none;
    }
}
.dropdown-divider {
    border-top: 1px solid #d62828;
}
.dropdown-item {
    color: #f77f00;
    &:hover {
        background-color: unset;
        color: unset;
    }
}

.navbar .container-fluid {
    justify-content: start;
}

.show-data {
    padding: 10px;
    background-color: white;
    height: fit-content;
}

/* Extra small devices (phones, 600px and down) */
@media only screen and (max-width: 600px) {
    .fixed-top {
        height: 155px;
    }
    .dropleft {
        margin-left: -20px;
    }
    .form-inline .form-control {
        width: 52%;
    }
    .navbar-brand {
        display: inline-block;
        text-align: center;
        margin: auto;
    }
    .show-data {
        margin-top: 155px;
        margin-bottom: -155px;
    }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 600px) {
    .form-inline .form-control {
        width: 70%;
        margin-left: 8px;
    }
    .navbar-brand {
        display: inline-block;
        padding-bottom: 15px;
        text-align: center;
        margin: auto;
    }
    .show-data {
        margin-top: 155px;
    }
}

/* Medium devices (landscape tablets, 768px and up) */
@media only screen and (min-width: 768px) {
    .dropleft {
        margin-left: 20px;
    }
    .form-inline .form-control {
        width: 84%;
        margin-left: 8px;
    }
    .navbar-brand {
        display: inline-block;
        padding-bottom: 15px;
        text-align: center;
        margin: auto;
    }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
    .dropleft {
        margin-left: 10px;
    }
    .form-inline .form-control {
        width: 86%;
    }
    .show-data {
        margin-top: 95px;
    }
}

/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (min-width: 1200px) {
    .dropleft {
        margin-left: 20px;
    }
    .form-inline .form-control {
        width: 89%;
    }
}
</style>
