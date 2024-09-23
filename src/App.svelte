<script>
 import { Router, Route, link } from 'svelte-routing';
 import Overview from './Overview.svelte';
 import userData from './movieData.json';
 import Edit from './Edit.svelte';
 import Add from './Add.svelte';
 import { onMount, onDestroy } from 'svelte';
import Goal from './goal.svelte';
import { VegaLite } from 'svelte-vega'; // Import Vega-Lite
import vegaEmbed from 'vega-embed';
// @ts-ignore
import moment from 'moment';

let happyCount=0;let sadCount=0;let amusedCount=0;let motivatedCount=0;let energeticCount=0;let lonelyCount=0;let afraidCount=0;let thrilledCount=0;
let avdCount =0;let comCount =0;let romCount =0;let famCount =0; let dramCount=0; let scifiCount =0; let horrorCount =0; let actCount =0;
let fantCount =0; let thrillCount =0;
let engCount =0;
let spanishCount=0;
let frenchCount =0;
let germanCount =0;
let hindiCount =0;
let teluguCount =0;
let chineseCount =0;
let koreanCount =0;
let japaneseCount =0;
let thaiCount =0;
let tamilCount =0;
let name ="Pranathi";
let currentIndex =0;
const entries = 3;
let isDisabled1 = false;
let isDisabled2 = true;
let showAddForm = false;
let activeCount =0;
let beginCount =0;
let showSettings = false;
let showEditForm = false;
let movieToEdit;
let settingsMenu;
let settingsButton;
let totalMovies = 0;

  function begin_Count(){
let startDate = (latestMovies[userData.user.length-1]).date;
console.log(startDate);

// Today's date
let today = new Date().toLocaleDateString('en-CA');
;
console.log(today);

// First, ensure you have Moment.js included in your project.
beginCount = moment(today).diff(moment(startDate), 'days');

  
}

  function active_Count(){
    let dateArray =[];
     for (let j=0;j< userData.user.length;j++){
    
    dateArray.push(userData.user[j].date);
    }
    let dateArray1 = [...new Set(dateArray)];
    activeCount = dateArray1.length;
   }
  
  function toggleLike(movieIndex) {
    latestMovies[movieIndex].like = latestMovies[movieIndex].like === "yes" ? "no" : "yes";
    // Optionally, trigger a state update if necessary
}

let showGoalSetter = false; // State to manage visibility of GoalAdder

    // Function to toggle visibility
    function toggleGoalAdder() {
        showGoalSetter = !showGoalSetter;
    }
let showOverview = false;
function toggleOverview(){
  showOverview = !showOverview;
}
let months =["Jan","Feb","March","April","May","June","July","August","September","October","November","December"];
function switchThemes(){

}
/*Function to update time dynamically*/
let dateValue='';
let day = new Date().getDate();
let month = months[new Date().getMonth()];
dateValue = day + " "+month;
let timeValue = new Date().toLocaleTimeString('en-CA');

  // Update date and time every second
  setInterval(() => {
    dateValue = dateValue;
    timeValue = new Date().toLocaleTimeString('en-CA');
  }, 1000);

function smoothScroll(target) {
  const element = document.querySelector(target);
  element.scrollIntoView({ behavior: 'smooth' });
}
function getSortedEntries(movies){
  return movies
      .slice()  // Create a copy of the array
      .sort((a, b) => {
          // Convert date strings to Date objects
          const dateA = new Date(a.date);
          const dateB = new Date(b.date);

          // Check if either date is invalid
          if (isNaN(dateA.getTime()) || isNaN(dateB.getTime())) {
              console.error('Invalid date encountered:', { dateA, dateB });
              // Handle invalid dates as needed, e.g., by treating them as the earliest dates
              return 0;  // Treat invalid dates as equal
          }

          // Compare timestamps
          return dateB.getTime() - dateA.getTime();  // Latest date first
      })

}
let latestMovies = getSortedEntries(userData.user);
function getNextMovie() {
  if (currentIndex + entries < latestMovies.length) {
      currentIndex += entries;
      isDisabled2 = false; // Enable the previous button if it was disabled
    }
    // Disable the next button if we’re at the end
    isDisabled1 = currentIndex + entries >= latestMovies.length;

}

  function getPrevMovie() {
    if (currentIndex > 0) {
      currentIndex -= entries;
      isDisabled1 = false; // Enable the next button if it was disabled
    }
    // Disable the previous button if we're at the start
    isDisabled2 = currentIndex <= 0;
  }

 
 

function addForm(){
showAddForm = !showAddForm;
}
function cancelAdd() {
    showAddForm = false; // Just close the form
  }


function showNotification() {
      // Check if the browser supports notifications
      if ("Notification" in window) {
        // Request permission from the user
        Notification.requestPermission().then(permission => {
          if (permission === "granted") {
            // Show the notification
            new Notification("Hi! User", {
              body: "Welcome to the website!",
            });
          }
        });
      } else {
        alert("This browser does not support notifications.");
      }
    }

    // Call the function to show notification when the page is loaded
    window.onload = showNotification;
    let selectedImage;
    let imageURL = '';


  function openEditForm(movie) {
    movieToEdit = movie;
    showEditForm = true;
    console.log(movie);
  }


  function upFeelings(){
    happyCount=0;
    sadCount=0;
    amusedCount=0;
    motivatedCount=0;
    energeticCount=0;
    lonelyCount=0;
    afraidCount=0;
    thrilledCount=0;

    for (let u = 0; u < userData.user.length; u++) {
    if (userData.user[u].feelings.happy) happyCount++;
    if (userData.user[u].feelings.sad) sadCount++;
    if (userData.user[u].feelings.energetic) energeticCount++;
    if (userData.user[u].feelings.motivated) motivatedCount++;
    if (userData.user[u].feelings.amused) amusedCount++;
    if (userData.user[u].feelings.fear) afraidCount++;
    if (userData.user[u].feelings.lonely) lonelyCount++;
    if (userData.user[u].feelings.thrilled) thrilledCount++;
  }

   emotionData = [
        { emotion: 'Happy', count: happyCount },
        { emotion: 'Sad', count: sadCount },
        { emotion: 'Energetic', count: energeticCount },
        { emotion: 'Motivated', count: motivatedCount },
        { emotion: 'Amused', count: amusedCount },
        { emotion: 'Afraid', count: afraidCount },
        { emotion: 'Lonely', count: lonelyCount },
        { emotion: 'Thrilled', count: thrilledCount }
    ];

  }

  function upGenre(){
    avdCount =0;
   comCount=0;
   romCount =0;
   famCount =0;
   dramCount =0;
   scifiCount =0;
   horrorCount =0;
   actCount =0;
   fantCount =0;
   thrillCount =0;

   for (let u = 0; u < userData.user.length; u++) {
    if (userData.user[u].genre.adventure) avdCount++;
    if (userData.user[u].genre.comedy) comCount++;
    if (userData.user[u].genre.romance) romCount++;
    if (userData.user[u].genre.family) famCount++;
    if (userData.user[u].genre.drama) dramCount++;
    if (userData.user[u].genre.sciFi) scifiCount++;
    if (userData.user[u].genre.horror) horrorCount++;
    if (userData.user[u].genre.action) actCount++;
    if (userData.user[u].genre.fantasy) fantCount++;
    if (userData.user[u].genre.thriller) thrillCount++;

  }

   genreData = [
        { genre: 'Adventure', count: avdCount },
        { genre: 'Comedy', count: comCount },
        { genre: 'Romance', count: romCount },
        { genre: 'Family', count: famCount },
        { genre: 'Drama', count: dramCount },
        { genre: 'Scifi', count: scifiCount },
        { genre: 'Horror', count: horrorCount },
        { genre: 'Action', count: actCount },
        { genre: 'Fantasy', count: fantCount },
        { genre: 'Thriller', count: thrillCount }
    ];

  }
  function upLanguage(){
   engCount =0;
   spanishCount=0;
   frenchCount =0;
   germanCount =0;
   hindiCount =0;
   teluguCount =0;
   chineseCount =0;
   koreanCount =0;
   japaneseCount =0;
   thaiCount =0;
   tamilCount =0;

   for (let u = 0; u < userData.user.length; u++) {
    if (userData.user[u].language === 'English') engCount++;
    if (userData.user[u].language ==='Spanish') spanishCount++;
    if (userData.user[u].language === 'French') frenchCount++;
    if (userData.user[u].language === 'German') germanCount++;
    if (userData.user[u].language === 'Hindi') hindiCount++;
    if (userData.user[u].language === 'Telugu') teluguCount++;
    if (userData.user[u].language ==='Chinese') chineseCount++;
    if (userData.user[u].language ==='Korean') koreanCount++;
    if (userData.user[u].language ==='Japanese') japaneseCount++;
    if (userData.user[u].language ==='Thai') thaiCount++;
    if (userData.user[u].language ==='Tamil') tamilCount++;

  }

   languageData = [
        { language: 'English', count: engCount },
        { language: 'Spanish', count: spanishCount },
        { language: 'French', count: frenchCount },
        { language: 'German', count: germanCount },
        { language: 'Hindi', count: hindiCount },
        { language: 'Telugu', count: teluguCount },
        { language: 'Chinese', count: chineseCount },
        { language: 'Korean', count: koreanCount },
        { language: 'Japanese', count: japaneseCount },
        { language: 'Thai', count: thaiCount },
        { language: 'Thai', count: tamilCount }

    ];



  }
  
  function saveMovie(updatedMovie) {
    const index = userData.user.findIndex(m => m.id === updatedMovie.id );

    // Update the movie in the userData array
    // Reduce counts for previous feelings
    if (index !== -1) {
      const movieTitle = updatedMovie.movieTitle;
      const dateWatched = updatedMovie.date;

      if (movieTitle && dateWatched) {
      userData.user[index] = updatedMovie;
      latestMovies = getSortedEntries(userData.user); // Refresh the movie list
    showEditForm = false;
    alert('Entry edited successfully');
    }
    else{
      alert('Please enter the required fields!!!');
    }
  }
   upFeelings();
   updateFeelingsChart();
   active_Count();

  upGenre();
  updateGenreChart();
  upLanguage();
  updateLanguageChart();
  getTotalMovies();

  }
  function addMovie(newMovie) {
    // Update the movie in the userData array
    //event.preventDefault(); // Prevent the default form submission

   const movieTitle = newMovie.movieTitle;
   const dateWatched = newMovie.date;

 if (movieTitle && dateWatched) {
//   // Your save logic here (e.g., pushing to an array, making an API call)
   alert('Movie saved successfully!'); // Show alert
    newMovie.id = userData.user.length+1;
     userData.user.push(newMovie)
     latestMovies = getSortedEntries(userData.user); // Refresh the movie list
     console.log(latestMovies);
     showAddForm = false;
 } else {
   alert('Please make sure that you have filled movie title and date.'); // Show alert for missing fields
 }
upFeelings();
updateFeelingsChart();
upGenre();
updateGenreChart();
upLanguage();
updateLanguageChart();
active_Count();
getTotalMovies();
}
  function cancelEdit() {
    showEditForm = false;
  }
  function navSettings(event){
    event.stopPropagation();
  showSettings = ! showSettings;
  console.log(showSettings);
  }
  function handleClickOutside(event) {
    if (settingsMenu && !settingsMenu.contains(event.target) && !settingsButton.contains(event.target)) {
      showSettings = false;
    }
  }
 function getTotalMovies(){
  totalMovies = userData.user.length;
 }
  onMount(() => {
    document.addEventListener('click', handleClickOutside);
    updateFeelingsChart();
    updateGenreChart();
    updateLanguageChart();
    active_Count();
    begin_Count();
    getTotalMovies();

  });

  onDestroy(() => {
    document.removeEventListener('click', handleClickOutside);
  });


  
  selectedImage = 'Adventure'; // Default image

  // Define image URLs
  const images = {
    Adventure: 'https://www.hdwallpapers.in/download/wild_adventure_4k-HD.jpg',
    Horror: 'https://th.bing.com/th/id/R.b77fe26a0d0f6d275a79aadf78753ca3?rik=FmwHqTQlXDsVGA&riu=http%3a%2f%2fwallpapercave.com%2fwp%2fLm6aayS.jpg&ehk=TDkQmzjWbqDrHpayPAaVjcAFTAAva%2bcI66mteq%2frqi4%3d&risl=&pid=ImgRaw&r=0',
    Action: 'https://wallpaperaccess.com/full/1810037.jpg',
    Romance:'https://images.pixexid.com/a-nighttime-sky-adorned-with-a-canopy-of-glowing-stars-where-fantastical-creatu-skan5pn3.jpeg',
  };

  // Background image based on selection
  $: bckimag = `url(${images[selectedImage]})`;



    

  for (let u = 0; u < userData.user.length; u++) {
    if (userData.user[u].feelings.happy) happyCount++;
    if (userData.user[u].feelings.sad) sadCount++;
    if (userData.user[u].feelings.energetic) energeticCount++;
    if (userData.user[u].feelings.motivated) motivatedCount++;
    if (userData.user[u].feelings.amused) amusedCount++;
    if (userData.user[u].feelings.fear) afraidCount++;
    if (userData.user[u].feelings.lonely) lonelyCount++;
    if (userData.user[u].feelings.thrilled) thrilledCount++;
  }
    
    let emotionData = [
        { emotion: 'Happy', count: happyCount },
        { emotion: 'Sad', count: sadCount },
        { emotion: 'Energetic', count: energeticCount },
        { emotion: 'Motivated', count: motivatedCount },
        { emotion: 'Amused', count: amusedCount },
        { emotion: 'Afraid', count: afraidCount },
        { emotion: 'Lonely', count: lonelyCount },
        { emotion: 'Thrilled', count: thrilledCount }
    ];

    for (let u = 0; u < userData.user.length; u++) {
    if (userData.user[u].genre.adventure) avdCount++;
    if (userData.user[u].genre.comedy) comCount++;
    if (userData.user[u].genre.romance) romCount++;
    if (userData.user[u].genre.family) famCount++;
    if (userData.user[u].genre.drama) dramCount++;
    if (userData.user[u].genre.sciFi) scifiCount++;
    if (userData.user[u].genre.horror) horrorCount++;
    if (userData.user[u].genre.action) actCount++;
    if (userData.user[u].genre.fantasy) fantCount++;
    if (userData.user[u].genre.thriller) thrillCount++;

  }


    let genreData = [
        { genre: 'Adventure', count: avdCount },
        { genre: 'Comedy', count: comCount },
        { genre: 'Romance', count: romCount },
        { genre: 'Family', count: famCount },
        { genre: 'Drama', count: dramCount },
        { genre: 'Scifi', count: scifiCount },
        { genre: 'Horror', count: horrorCount },
        { genre: 'Action', count: actCount },
        { genre: 'Fantasy', count: fantCount },
        { genre: 'Thriller', count: thrillCount }
    ];

    for (let u = 0; u < userData.user.length; u++) {
    if (userData.user[u].language === 'English') engCount++;
    if (userData.user[u].language ==='Spanish') spanishCount++;
    if (userData.user[u].language === 'French') frenchCount++;
    if (userData.user[u].language === 'German') germanCount++;
    if (userData.user[u].language === 'Hindi') hindiCount++;
    if (userData.user[u].language === 'Telugu') teluguCount++;
    if (userData.user[u].language ==='Chinese') chineseCount++;
    if (userData.user[u].language ==='Korean') koreanCount++;
    if (userData.user[u].language ==='Japanese') japaneseCount++;
    if (userData.user[u].language ==='Thai') thaiCount++;
    if (userData.user[u].language ==='Tamil') tamilCount++;

  }

    let languageData = [
        { language: 'English', count: engCount },
        { language: 'Spanish', count: spanishCount },
        { language: 'French', count: frenchCount },
        { language: 'German', count: germanCount },
        { language: 'Hindi', count: hindiCount },
        { language: 'Telugu', count: teluguCount },
        { language: 'Chinese', count: chineseCount },
        { language: 'Korean', count: koreanCount },
        { language: 'Japanese', count: japaneseCount },
        { language: 'Thai', count: thaiCount },
        { language: 'Thai', count: tamilCount }

    ];
  console.log(languageData);


  function updateFeelingsChart()
  {
    let barChartSpec = {
      width: 400,
      height: 300,
      mark: {
      type: "bar",
      tooltip: true,
    },
      encoding: {
        x: { field: "emotion", type: "nominal", axis: { labelAngle: -45, title:"Feelings" } },
        y: { field: "count", type: "quantitative",axis: { title: "Number of movies" } },
        color: { value: "#d0ba1c" },
      },
      data: { values: emotionData },
      config: {
    background: "white" // Set background color to black
  }
    };
   
    // @ts-ignore
    vegaEmbed('#chart', barChartSpec).catch(console.error);
  }

  function updateLanguageChart(){
    let barChartSpec = {
      width: 400,
      height: 300,
      mark: {
      type: "bar",
      tooltip: true,
    },
      encoding: {
        x: { field: "language", type: "nominal", axis: { labelAngle: -45, title:"Languages" } },
        y: { field: "count", type: "quantitative",axis: { title: "Number of movies" } },
        color: { value: "#d0ba1c" },
      },
      data: { values: languageData },
      config: {
    background: "white" // Set background color to black
  }
    };
   
    // @ts-ignore
    vegaEmbed('#chart3', barChartSpec).catch(console.error);

  }

  function updateGenreChart()
  {
    let barChartSpec = {
      width: 400,
      height: 300,
      mark: {
      type: "bar",
      tooltip: true,
    },
      encoding: {
        x: { field: "genre", type: "nominal", axis: { labelAngle: -45, title:"Genre" } },
        y: { field: "count", type: "quantitative",axis: { title: "Number of movies" } },
        color: { value: "#d0ba1c" },
      },
      data: { values: genreData },
      config: {
    background: "white" // Set background color to black
  }
    };
   
    // @ts-ignore
    vegaEmbed('#chart2', barChartSpec).catch(console.error);
  }

  

  function deleteMovie(movie1) {
    // Find the index of the movie to be deleted
    const index = userData.user.findIndex(movie => movie.movieTitle === movie1.movieTitle);

    if (index !== -1) {
      // Remove the movie from the userData array
      userData.user.splice(index, 1);
      latestMovies = getSortedEntries(userData.user); // Refresh the movie list
      alert('Movie deleted successfully');
     upFeelings();
      // Recalculate counts for the feelings/emotions chart
      updateFeelingsChart();
      active_Count(); // Update the active count of unique days
      upGenre();
      updateGenreChart();
      upLanguage();
      updateLanguageChart();
      getTotalMovies();
    }
  }

    
</script>


<main style="background-image:{bckimag};">
  <div class="backImage"></div>
  <nav>
      <a use:link href="#section1" on:click="{() => smoothScroll('#section1')}"> HOME</a>
      <a use:link href="#section2"on:click="{() => smoothScroll('#section2')}"> OVERVIEW</a>
      <div class="navInputs">
      <span class="date">{dateValue}</span>
      <button class="settings-button" on:click={navSettings} bind:this={settingsButton}>⚙️</button>
      {#if showSettings}
    <div class="settings-menu" bind:this={settingsMenu}>
      <label>
        Change Themes<br>
        <label><input type='radio' name="settings" bind:group={selectedImage} value='Adventure'> Adventure</label><br>
        <label><input type='radio'name="settings"  bind:group={selectedImage} value='Horror'> Horror</label><br>
        <label><input type='radio' name="settings" bind:group={selectedImage} value='Action'> Action</label><br>
        <label><input type='radio' name="settings" bind:group={selectedImage} value='Romance'> Romance</label><br>
      </label>
     
      <!-- Add more options as needed -->
    </div>
  {/if}

        
      </div>
  </nav>
  <div id ="section1" class="content section1">
    <h1 class="name">WELCOME {name.toUpperCase()} &#128075;<br> </h1>
    <div class="activeBegin"> <p class="active">ACTIVE FOR {activeCount} DAYS</p>
      <p class="begin"> {beginCount} DAYS SINCE BEGINNING</p><br>
  </div>
    <p>MOVIE ENTRIES  <button class="normalButton" on:click="{() => smoothScroll('#section2')}" on:click={addForm}>ADD</button></p>
    
    {#if latestMovies.length > 0}
      <div class="movie-container">
        <button class ="next normalButton"on:click={getPrevMovie} disabled={isDisabled2}>&#9664;</button>
        {#each latestMovies.slice(currentIndex, currentIndex + entries) as movie,movieIndex}
        <div class="movie-item">
          {#if movie.image === ''}
          <img src='https://www.bing.com/th?id=OIP.DAuF8ksdA5Kjh7fLifDpnwHaHa&w=150&h=150&c=8&rs=1&qlt=90&o=6&dpr=1.3&pid=3.1&rm=2' alt={movie.movieTitle} width="200" height="170">
        {:else}
          <img src={movie.image} alt={movie.movieTitle} width="200" height="170">
        {/if}
        
          <div class="movie-details">
            <h4>{movie.movieTitle} ({#if movie.releaseYear===""}year{:else}{movie.releaseYear}{/if})</h4>
            <p>Date Watched: {movie.date}</p>
            <p>Language: {#if movie.language===""} Unknown language {:else} {movie.language}{/if}</p>
            <p>Duration: {#if movie.duration ===""} Unknown duration {:else}{movie.duration}{/if}</p>
            <p>Genres: <br>
              {#if movie.genre.adventure} Adventure {/if}
              {#if movie.genre.comedy} Comedy {/if}
              {#if movie.genre.romance} Romance{/if}
              {#if movie.genre.family} Family {/if}
              {#if movie.genre.drama} Drama {/if}
              {#if movie.genre.sciFi} Sci-Fi {/if}
              {#if movie.genre.action} Action {/if}
              {#if movie.genre.horror}Horror {/if}
              {#if movie.genre.thriler}Thriller {/if}
              {#if movie.genre.fantasy}Fantasy{/if}
              {#if !movie.genre.adventure && !movie.genre.comedy && !movie.genre.romance && !movie.genre.family && !movie.genre.drama && ! movie.genre.sciFi
              && !movie.genre.action && !movie.genre.horror && !movie.genre.thriler && !movie.genre.fantasy} Unknown genre {/if}
              
            </p>
            <p class="movieLike {movie.like === "yes"? 'like' : 'not-like'}"> {#if movie.like==="yes"}❤️ {:else} 🤍{/if} </p>

              <button class="normalButton edit" on:click={() => openEditForm(movie)} on:click="{() => smoothScroll('#section3')}">EDIT</button>
              <button class="normalButton edit"on:click={() => deleteMovie(movie)} >DELETE</button>

          </div>
        </div>
      {/each}      
      <button class="next normalButton" on:click={getNextMovie} disabled={isDisabled1}>&#9654;</button>
      </div>
    {:else}
      <p>No movies found.</p>
    {/if}
    
    <div class="spacer"></div>
</div>
<div id ="section2" class="content section2 ">
<div>
  {#if showAddForm}
      <Add onSave={addMovie} onCancel={cancelAdd} />
    {/if}
</div><br><br>
<div id="section3">
  {#if showEditForm}
  <Edit movie={movieToEdit} onSave={saveMovie} onCancel={cancelEdit} />
  {/if}
</div>
<br>
<div>
  <button class="normalButton goalButton" on:click={toggleGoalAdder}>
      {showGoalSetter ? 'HIDE GOALS' : 'ADD GOALS'}
  </button>
<br><br>
<div class:visible={showGoalSetter} class="goal-container">
  <Goal />
</div>
 
</div>
<br><br>
  <div id="section4">
    <button class="normalButton goalButton" on:click={toggleOverview} on:click="{() => smoothScroll('#overview')}" on:click={updateFeelingsChart} on:click={updateGenreChart} on:click={updateLanguageChart} on:click={getTotalMovies}>
      {showOverview ? 'HIDE OVERVIEW' : 'SHOW OVERVIEW'}
  </button>
<br><br>
  {#if showOverview}
  <div id="overview">
    <p>TOTAL NUMBER OF MOVIES WATCHED : {totalMovies}</p>
    <div id="chart"></div>
    <div id="chart2"></div>
    <div id="chart3"></div>
  </div>
  
  {/if}

    
  </div>
 
</div>


</main>
<style>
/* .time{
  width:200px;
  border-radius: 20px;
  color:white;
  font-size: 20px;
  height: 20px;
} */
/* .date{
  height:20px;
  padding-left: 1000px;
  border-radius: 20px;
  color:white;
  font-size: 20px;
} */
nav {
  position: relative;
  z-index: 1;
  background-color:white;
  height: 60px;
  width: 100%;
  display: flex;
  align-items: center;
 /* Distributes space between items */
  padding: 0 20px; /* Add padding to prevent items from touching the edges */
  opacity: 1;
  font-weight: bold;
  border-color: black;
}

a {
  color: black;
  padding: 30px 10px;
  text-decoration: none;
  display: flex;
}

a:hover {
  background-color: white;
  cursor: pointer;
  height:60px;
}

.navInputs {
  display: flex;
  align-items: center;
}

.date {
  margin-left: 20px; /* Space between date and time */
  color: black;
  font-size: 15px;
  width:150px;
  margin-right: 0px;
}

.settings-button {
  background: none;
  border: none;
  color: white;
  font-size: 24px;
  cursor: pointer;
  position: relative;
  top:5px;
}

.settings-button:hover {
  color:white; /* Change color on hover */
}

.content {
    padding: 20px;
  }

.spacer {
    height: 250px; /* Adds space to scroll */
}
.name{
  
 font-size: 50px;
 font-family: cursive;
 text-align: center;
}
.movie-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px; /* Adjust the space between items */
  color:black;
}

.movie-item {
  flex: 1;
  min-width: 300px;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 10px;
  background-color: #f9f9f9;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  height: 450px;
}

.movie-details {
  margin-left: 10px;
}
.next{
  height:40px;
  margin-top:250px;
  
}
.edit{
  position: relative;
  margin-right:10px;
}
/* .addform-header,.movie-form{
  color:black ;
  align-items: flex-end;
  font-size: 30px;
  background-color: white;
  border-radius: 20px;
  padding-left: 20px;

  
} */

.movieLike{
  cursor:pointer;
  font-size: 1.5rem;
}
.navInputs{
  display: flex;
  height:20px;
  padding-left: 1020px;
  border-radius: 20px;
  color:white;
  font-size: 20px;
  text-align: center;
  align-items: end;
}
.like{
  color:red;
}
.not-like{
  color:gray;
}
.settings-menu {
    position: absolute;
    top: 50px; /* Adjust based on your layout */
    right: 0;
    background: white;
    border: 1px solid #ddd;
    padding: 10px;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
    /* Add more styles to match your design */
    color:black;
    text-align: justify;
  }
  main {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow-y: auto;
  font-family:cursive;
  background-repeat: no-repeat;
  background-size: cover;
  /* background-image: url('https://th.bing.com/th/id/R.b77fe26a0d0f6d275a79aadf78753ca3?rik=FmwHqTQlXDsVGA&riu=http%3a%2f%2fwallpapercave.com%2fwp%2fLm6aayS.jpg&ehk=TDkQmzjWbqDrHpayPAaVjcAFTAAva%2bcI66mteq%2frqi4%3d&risl=&pid=ImgRaw&r=0'); */
  /* background-image: url('https://static.vecteezy.com/system/resources/previews/023/218/138/non_2x/black-starry-sky-dark-night-sky-infinity-space-with-shiny-stars-mystery-dark-universe-background-vector.jpg');  */
}

.goalButton{
  height:100px;
  width:500px;
  margin-left: 500px;
  border-color:whitesmoke;
  background-color: white;
  color:black;
  font-size: 20px;

}
.goalButton:hover{
  background-color: #d0ba1c;
}
#overview{
  background-color: white;
  width:100%;
  min-height:300px;
  border-radius: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 10px;
  padding-right: 10px;
}
.goal-container {
    display: none;
  }
  
  .goal-container.visible {
    display: block;
  }
  #chart{
    margin-left:50px;
  }
  #chart2{
    margin-left: 300px;
  }
  #chart3{
    display: flex;
    justify-content: center;
  }
  .activeBegin{
    display: flex;
    justify-content: center;
  }
  .active{
    background-color:#d0ba1c;
    border-color: white;
    border-radius: 20px;
    border-width: 10px;
    height:60px;
    width:200px;
    text-align: center;
    font-size: 20px;

  }
  .begin{
    margin-left: 10px;
    background-color:#d0ba1c;
    border-color: white;
    border-radius: 20px;
    border-width: 10px;
    height:60px;
    width:200px;
    text-align: center;
    font-size: 20px;

  }





</style>
  
