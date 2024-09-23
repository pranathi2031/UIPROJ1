<script>
    export let movie; // Movie object to edit
    export let onSave; // Callback function to save the changes
    export let onCancel; // Callback function to cancel the edit
    import { onMount } from 'svelte';
  
    // Local state for editing
    let editedMovie = {};      
    editedMovie = {...movie};
    

    // Create a copy to edit
    function save() {
      onSave(editedMovie); // Call the save function with the edited movie
    }

    function handleFileUpload(event) {
  const file = event.target.files[0];

  if (file && file.type.startsWith('image/')) {
    const reader = new FileReader();

    reader.onload = () => {
      if (typeof reader.result === 'string') {
        // If it's a base64 string (result of readAsDataURL)
        console.log('Base64 String:', reader.result);
        editedMovie.image = reader.result; // base64-encoded string
      } else if (reader.result instanceof ArrayBuffer) {
        // If it's an ArrayBuffer (result of readAsArrayBuffer)
        console.log('ArrayBuffer:', reader.result);
        
        // Convert ArrayBuffer to a base64 string (optional, if needed)
        const base64String = arrayBufferToBase64(reader.result);
        editedMovie.image = base64String; // Save base64 string
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
  
  <div class="editForm">
    <h3>Edit Movie</h3>
    <label>Movie Title:
      <input type="text" bind:value={editedMovie.movieTitle} required>
    </label><br><br>
    
    <label>Date Watched:
      <input type="date" bind:value={editedMovie.date} required>
    </label><br><br>
    
    <!-- Additional fields similar to the add form -->
    <label aria-required="true">Genre of the movie:
      <div class="checkbox-container">
        <div class="left">
          <label><input type="checkbox" bind:checked={editedMovie.genre.adventure}> Adventure</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.comedy}> Comedy</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.romance}> Romance</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.family}> Family</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.drama}> Drama</label><br>
        </div>
        <div class="right">
          <label><input type="checkbox" bind:checked={editedMovie.genre.sciFi}> Sci-Fi</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.horror}> Horror</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.action}> Action</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.fantasy}> Fantasy</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.genre.thriller}> Thriller</label><br>
        </div>
      </div>
      
      </label><br>
      <label>How do you feel after watching the movie:
      <div class ="checkbox-container">
        <div class="left">
          <label><input type="checkbox" bind:checked={editedMovie.feelings.happy}> Happy</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.feelings.sad}> Sad</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.feelings.amused}> Amused</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.feelings.energetic}> Energetic</label><br>
        </div>
        <div class="right">
          <label><input type="checkbox" bind:checked={editedMovie.feelings.motivated}>Motivated</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.feelings.fear}>Afraid</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.feelings.lonely}> Lonely</label><br>
          <label><input type="checkbox" bind:checked={editedMovie.feelings.thrilled}>Thrilled</label><br>
        </div>
      </div>
      </label><br>
  
        <label> Language:
          <select id="languageSelect" class="language-select" bind:value={editedMovie.language} required>
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
    
            <!-- Add more options as needed -->
          </select>
        </label><br><br>

      <label>Do you like the movie? <br>
        <label>
            <input type="radio" name="like" bind:group={editedMovie.like} value="yes"> Yes
        </label><br>
        <label>
            <input type="radio" name="like" bind:group={editedMovie.like} value="no"> No
        </label>
    </label><br><br>
    <label> Release Year:
      <input type="number" min="2000" max="2024" bind:value={editedMovie.releaseYear}>
    </label><br><br>
  
    <label> Duration:
      <input type="text" bind:value={editedMovie.duration}>
    </label><br><br>

    <label>Rating:<br>
      
      <label><input type="radio" name="rating1" bind:group={editedMovie.rating} value=1> 1</label><br>
      <label><input type="radio" name="rating2" bind:group={editedMovie.rating} value=2> 2</label><br>
      <label><input type="radio" name="rating3" bind:group={editedMovie.rating} value=3> 3</label><br>
      <label><input type="radio" name="rating4" bind:group={editedMovie.rating} value=4> 4</label><br>
      <label><input type="radio" name="rating5" bind:group={editedMovie.rating} value=5> 5</label><br>
    </label><br><br>
    <label>Review:<br>
    <br><textarea bind:value={editedMovie.review}></textarea ></label><br><br>

    <label> Change Image:    
        <input type="file" accept="image/*" on:change={handleFileUpload}>
    </label><br><br>



    <button class="normalButton"on:click={save}>SAVE</button>
    <button class="normalButton" on:click={onCancel}>CLOSE</button>
  </div>
  
  <style>
    /* Add styles as needed */
    .editForm{
  border-color: black;
  border-radius: 20px;
  border-width: 30px;
  width:95%;
  font-size: 30px;
  color:black ;
  align-items: flex-end;
  font-size: 20px;
  background-color: white;
  border-radius: 50px;
  padding-left: 20px;
  padding-right: 20px;
  padding-bottom: 20px;
  padding-top: 20px;
  /* background-image: url('https://static.vecteezy.com/system/resources/previews/000/543/795/original/gray-poster-abstract-background-vector.jpg'); */
  background-size: cover;
  background-repeat: no-repeat;
  margin-left: 30px;
  margin-right: 20px;

  
}
h3{
  text-align: center;
}

.checkbox-container {
  display: flex;
  justify-content:space-evenly; /* Space between the two columns */
  width: 100%; /* Adjust as necessary */
}

.left{
  width: 25%; /* Each side takes up half the space */
}
.right{
  width: 85%;
}
input,input[type="checkbox"]{
  font-size: 20px;
  font-family: cursive;
}
#languageSelect{
  font-size: 20px;
  font-family: cursive;
  width: 400px;}

  textarea{
  height: 300px;
  width:500px;
  margin-left:10px;
}

input:required {
  border: 2px solid red; /* Change the border color */
  background-color: #ffe6e6; /* Light red background */
}

input:valid{
  background-color: white;
  border-color: black;
}




  </style>
  