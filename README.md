<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Report Incident</title>
    <script>
        // Function to get the current location
        function getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(showPosition, showError);
            } else {
                alert("Geolocation is not supported by this browser.");
            }
        }

        // Function to display the position
        function showPosition(position) {
            const lat = position.coords.latitude;
            const lon = position.coords.longitude;

            // Generate Google Maps link
            const googleMapsLink = `https://www.google.com/maps?q=${lat},${lon}`;

            // Display the Google Maps link in the text field
            document.getElementById('location').value = googleMapsLink;

            // Update the WhatsApp link with the Google Maps URL
            updateWhatsAppLink(googleMapsLink);
        }

        // Function to handle geolocation errors
        function showError(error) {
            switch (error.code) {
                case error.PERMISSION_DENIED:
                    alert("User denied the request for Geolocation.");
                    break;
                case error.POSITION_UNAVAILABLE:
                    alert("Location information is unavailable.");
                    break;
                case error.TIMEOUT:
                    alert("The request to get user location timed out.");
                    break;
                case error.UNKNOWN_ERROR:
                    alert("An unknown error occurred.");
                    break;
            }
        }

        // Function to update the WhatsApp link with form information
        function updateWhatsAppLink(location) {
            const who = document.getElementById('who').value;
            const what = document.getElementById('what').value;
            const where = document.getElementById('where').value;
            const when = document.getElementById('when').value;
            const actionTaken = document.getElementById('actionTaken').value;

            const message = `Location: ${location}%0AWho: ${who}%0AWhat: ${what}%0AWhere: ${where}%0AWhen: ${when}%0AAction Taken: ${actionTaken}`;
            const phoneNumber = '+9647845301590';
            const whatsappURL = `https://api.whatsapp.com/send?phone=${phoneNumber}&text=${message}`;

            // Update the WhatsApp link
            document.getElementById('whatsappLink').href = whatsappURL;
        }

        // Function to be called when the form is submitted
        function sendWhatsAppMessage() {
            const location = document.getElementById('location').value;
            if (!location) {
                alert('Location is not available. Please try again.');
                return;
            }

            // Update the WhatsApp link with the form information
            updateWhatsAppLink(location);

            // Open the WhatsApp link in a new tab
            window.open(document.getElementById('whatsappLink').href, '_blank');
        }

        // Get location on page load
        window.onload = function() {
            getLocation();
            document.getElementById('whatsappLink').addEventListener('click', sendWhatsAppMessage);
        };
    </script>
</head>
<body>
    <form onsubmit="return false;">
        <label for="location">Location (Google Maps Link):</label><br>
        <input type="text" id="location" name="location" readonly><br><br>
        
        <label for="who">Who?</label><br>
        <input type="text" id="who" name="who"><br><br>
        
        <label for="what">What?</label><br>
        <textarea id="what" name="what"></textarea><br><br>
        
        <label for="where">Where?</label><br>
        <textarea id="where" name="where"></textarea><br><br>
        
        <label for="when">When?</label><br>
        <input type="datetime-local" id="when" name="when"><br><br>
        
        <label for="actionTaken">Action Taken?</label><br>
        <textarea id="actionTaken" name="actionTaken"></textarea><br><br>
        
        <a id="whatsappLink" href="#" onclick="sendWhatsAppMessage()">Send WhatsApp Message</a>
    </form>
</body>
</html>
