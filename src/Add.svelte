<script>
    export let onSave;
    export let onCancel;
    let yearSelect = false;
    let durationSelect = false;
    let platformSelect = false;
    let ratingSelect = false;
    let reviewSelect = false;
    let imageSelect = false; 
    let imageSrc = new ArrayBuffer(64);  // This will hold the base64 or URL of the uploaded image
    let imageFile = null;
     let newMovie = {
      id:'',
      date: new Date().toLocaleDateString('en-CA'),
      movieTitle: "",
      language: "",
      genre: {
        adventure: false,
        comedy: false,
        romance: false,
        family: false,
        drama: false,
        sciFi: false,
        horror: false,
        action: false,
        fantasy: false,
        thriller: false,
      },
      releaseYear: "",
      duration: "",
      rating: 1,
      review: "",
      feelings: {
        happy: false,
        sad: false,
        amused: false,
        energetic: false,
        motivated: false,
        fear: false,
        lonely: false,
        thrilled: false,
      },
      like: 'no',
      image: ""
    };
  
function save() {
  onSave(newMovie);
 }


function addYear(){
  yearSelect =!yearSelect;
}

function addDuration(){
  durationSelect =!durationSelect;
}
function addPlatform(){
  platformSelect =!platformSelect;
}
function addRating(){
ratingSelect =!ratingSelect;
}
function addReview(){
  reviewSelect =!reviewSelect;
}
function addImage(){
  imageSelect = !imageSelect;
}

function handleFileUpload(event) {
  const file = event.target.files[0];

  if (file && file.type.startsWith('image/')) {
    const reader = new FileReader();

    reader.onload = () => {
      if (typeof reader.result === 'string') {
        // If it's a base64 string (result of readAsDataURL)
        console.log('Base64 String:', reader.result);
        newMovie.image = reader.result; // base64-encoded string
      } else if (reader.result instanceof ArrayBuffer) {
        // If it's an ArrayBuffer (result of readAsArrayBuffer)
        console.log('ArrayBuffer:', reader.result);
        
        // Convert ArrayBuffer to a base64 string (optional, if needed)
        const base64String = arrayBufferToBase64(reader.result);
        newMovie.image = base64String; // Save base64 string
      }
    };

    // Use readAsDataURL() if you want a base64 string to display images
    reader.readAsDataURL(file);

    // Or use readAsArrayBuffer() if you need the raw binary data
    // reader.readAsArrayBuffer(file);  // Uncomment if you want raw data
  }
}

// Helper function to convert ArrayBuffer to base64 string (optional)
function arrayBufferToBase64(buffer) {
  let binary = '';
  let bytes = new Uint8Array(buffer);
  let len = bytes.byteLength;
  for (let i = 0; i < len; i++) {
    binary += String.fromCharCode(bytes[i]);
  }
  return window.btoa(binary);
}

</script>
  
  <div class="addForm">
    <div class="addform-header">
      <h3>Add Movie </h3>
    </div>
    <form class="movie-form">
      <label>Movie Title:
        <input type="text" bind:value={newMovie.movieTitle} required>
      </label><br><br>
      <label>Date Watched:
        <input type="date" bind:value={newMovie.date} required>
      </label><br><br>
      <label>Genre of the movie:
        <div class="checkbox-container">
          <div class="left">
            <label><input type="checkbox" bind:checked={newMovie.genre.adventure}> Adventure</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.comedy}> Comedy</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.romance}> Romance</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.family}> Family</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.drama}> Drama</label><br>
          </div>
          <div class="right">
            <label><input type="checkbox" bind:checked={newMovie.genre.sciFi}> Sci-Fi</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.horror}> Horror</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.action}> Action</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.fantasy}> Fantasy</label><br>
            <label><input type="checkbox" bind:checked={newMovie.genre.thriller}> Thriller</label><br>
          </div>
        </div>
        
        </label><br>
        <label>How do you feel after watching the movie:
        <div class="checkbox-container">
          <div class="left">
            <label><input type="checkbox" bind:checked={newMovie.feelings.happy}> Happy</label><br>
            <label><input type="checkbox" bind:checked={newMovie.feelings.sad}> Sad</label><br>
            <label><input type="checkbox" bind:checked={newMovie.feelings.amused}> Amused</label><br>
            <label><input type="checkbox" bind:checked={newMovie.feelings.energetic}> Energetic</label><br>
          </div>
          <div class="right">
            <label><input type="checkbox" bind:checked={newMovie.feelings.motivated}>Motivated</label><br>
            <label><input type="checkbox" bind:checked={newMovie.feelings.fear}>Afraid</label><br>
            <label><input type="checkbox" bind:checked={newMovie.feelings.lonely}> Lonely</label><br>
            <label><input type="checkbox" bind:checked={newMovie.feelings.thrilled}>Thrilled</label><br>
          </div>
        </div>
          </label><br>
    
        <label> Language:
          <select id="languageSelect" class="language-select" bind:value={newMovie.language} required>
            <option value="" disabled selected>--Please select a language--</option>
            <option value="English">English</option>
            <option value="Spanish">Spanish</option>
            <option value="French">French</option>
            <option value="German">German</option>
            <option value="Hindi">Hindi</option>
            <option value="Telugu">Telugu</option>
            <option value="Chinese">Chinese</option>
            <option value="Korean">Korean</option>
            <option value="Japanese">Japanese</option>
            <option value="Thai">Thai</option>
            <option value="Tamil">Tamil</option>
    
          </select>
        </label><br><br>
        <label>Do you like the movie? 
          <label>
              <input type="radio" name="like" bind:group={newMovie.like} value="yes"> Yes
          </label>
          <label>
              <input type="radio" name="like" bind:group={newMovie.like} value="no"> No
          </label>
      </label><br><br>
      
      <label><button type="button" class="normalButton redPlus" on:click={addYear}>{#if !yearSelect}+ {:else} - {/if}</button> Release Year
        {#if yearSelect}: 
        <input type="number" min="2000" max="2024" bind:value={newMovie.releaseYear}>{/if}
      </label><br><br>
    
      <label><button type="button" class="normalButton redPlus" on:click={addDuration}>{#if !durationSelect}+ {:else} - {/if}</button> Duration
        {#if durationSelect} : <input type="text"placeholder="example 1h36m" bind:value={newMovie.duration}>{/if}
      </label><br><br>
      
      <label><button type="button" class="normalButton redPlus" on:click={addRating}>{#if !ratingSelect}+ {:else} - {/if}</button> Rating
        {#if ratingSelect} :
        <br><label><input type="radio" name="rating1" bind:group={newMovie.rating} value=1> 1</label><br>
        <label><input type="radio" name="rating2" bind:group={newMovie.rating} value=2> 2</label><br>
        <label><input type="radio" name="rating3" bind:group={newMovie.rating} value=3> 3</label><br>
        <label><input type="radio" name="rating4" bind:group={newMovie.rating} value=4> 4</label><br>
        <label><input type="radio" name="rating5" bind:group={newMovie.rating} value=5> 5</label><br>
        {/if}
      </label><br><br>
      <label><button type="button" class="normalButton redPlus" on:click={addReview}>{#if !reviewSelect}+ {:else} - {/if}</button> Review
        {#if reviewSelect}:<br><br><textarea bind:value={newMovie.review}></textarea >{/if}</label><br><br>

      <label><button type="button" class="normalButton redPlus" on:click={addImage}>{#if !imageSelect}+ {:else} - {/if}</button> Image URL 
      <div class:visible={imageSelect} class="image-container"> :    
      <input type="file" accept="image/*" on:change={handleFileUpload}>
      </div>
      </label>
      <br><br>
      <button class="normalButton" on:click={save}>SAVE</button>
      <button class="normalButton" on:click={onCancel}>CLOSE</button> <br>
    </form>
    <br>
  </div>
    
 

  
  <style>
  .addForm{
  border-color: black;
  border-radius: 50px;
  border-width: 30px;
  width:95%;
  font-size: 20px;
  color:black ;
  align-items: flex-end;
  background-color: white;
  padding-left: 20px;
  padding-right: 20px;
  background-size: cover;
  background-repeat: no-repeat;
  margin-left: 30px;
  margin-right: 20px;

  
}
input,input[type="checkbox"]{
  font-size: 18px;
  font-family: cursive;
}
h3{
  text-align: center;
  font-size:30px;
}
  
#languageSelect{
  font-size: 18px;
  font-family: cursive;
  width: 400px;}

  .redPlus {
  font-size: 20px;
  color: white;
  padding-left: 10px;
  padding-right: 10px;
  border: none;
}
.checkbox-container {
  display: flex;
  justify-content:space-evenly; /* Space between the two columns */
  width: 100%; 
}

.left{
  width: 25%; /* Each side takes up half the space */
}
.right{
  width: 85%;
}


input:required {
  border: 2px solid red; 
  background-color: #ffe6e6; 
}

input:valid{
  background-color: white;
  border-color: black;
}

textarea{
  height: 300px;
  width:500px;
  margin-left:30px;
  font-family: cursive;
  font-size: 18px;
}
.image-container {
    display: none;
    margin-left: 5px;
  }
  
.image-container.visible {
    display: inline;
}

</style>
  