<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Starlight International School – Login / Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
body {
    margin: 0;
    padding: 0;
    font-family: "Poppins", sans-serif;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Login Box */
.login-box {
    width: 90%;
    max-width: 500px;
    background: linear-gradient(135deg, #4a0eff, #c400ff, #ff2f7a);
    padding: 40px;
    border-radius: 20px;
    text-align: center;
    color: white;
    box-shadow: 0 4px 25px rgba(0,0,0,0.3);
}
.login-box h2 {
    font-size: 28px;
    margin-bottom: 25px;
    font-weight: 600;
}
input, select {
    width: 100%;
    padding: 15px;
    border-radius: 10px;
    border: none;
    outline: none;
    margin-bottom: 20px;
    font-size: 16px;
}
button {
    width: 100%;
    padding: 15px;
    background: #00ff99;
    color: black;
    border: none;
    border-radius: 10px;
    font-size: 18px;
    font-weight: 600;
    cursor: pointer;
    margin-top: 10px;
}
button:hover { background: #00d480; }
.error { margin-top: 15px; color: #ffcccc; font-size: 15px; display: none; }

/* Dashboard */
.dashboard {
    display: none;
    flex-direction: column;
    width: 100%;
    min-height: 100vh;
    background-color: #800080; /* Purple background */
    padding-bottom: 50px;
}
.header {
    text-align: center;
    padding: 30px 0;
    font-size: 34px;
    font-weight: 700;
    color: white;
    background: linear-gradient(135deg, #4a0eff, #c400ff, #ff2f7a);
}
.wrapper {
    width: 90%;
    max-width: 1500px;
    margin: auto;
}
.form-card, .record-card {
    background: white;
    padding: 40px;
    border-radius: 16px;
    box-shadow: 0 8px 30px rgba(0,0,0,0.2);
    margin-bottom: 50px;
}
.form-grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    column-gap: 1cm;
    row-gap: 1cm;
}
label { font-weight: 600; margin-bottom: 6px; display: block; }
table { width: 100%; border-collapse: collapse; }
th, td { border: 1px solid #ccc; padding: 10px; text-align: left; }
th { background: #f2f2f2; font-weight: 600; }
.record button { background: #ff4b4b; padding: 8px 15px; border-radius: 6px; border: none; cursor: pointer; color: white; }
.record button:hover { background: #d60000; }
#logoutBtn, #exportBtn {
    width: auto;
    padding: 10px 25px;
    margin-bottom: 20px;
    background: #ff4b4b;
    color: white;
    font-weight: 600;
    border-radius: 10px;
    cursor: pointer;
    margin-right: 10px;
}
#logoutBtn:hover, #exportBtn:hover { background: #d60000; }
</style>
</head>
<body>

<!-- Login Box -->
<div class="login-box" id="loginBox">
    <h2>🌟 Starlight International School</h2>
    <h3 style="font-size: 18px; margin-bottom: 25px;">Admin Login</h3>

    <input type="text" id="username" placeholder="Enter Username">
    <input type="password" id="password" placeholder="Enter Password">
    <button onclick="login()">Login</button>
    <div id="error" class="error">Invalid username or password</div>
</div>

<!-- Dashboard -->
<div class="dashboard" id="dashboard">
    <div class="header">🌟 Starlight International School – Admission Dashboard</div>
    <div class="wrapper">
        <button id="logoutBtn" onclick="logout()">Logout</button>
        <button id="exportBtn" onclick="exportToExcel()">📥 Export to Excel</button>

        <div class="form-card">
            <h2 style="margin-bottom: 25px;">Student Admission Form</h2>
            <form id="admissionForm">
                <div class="form-grid">
                    <div>
                        <label>Admission Number</label>
                        <input type="text" id="admissionNumber" value="10001" readonly>
                    </div>
                    <div><label>Student Name</label><input type="text" id="name" placeholder="Enter name" required></div>
                    <div><label>Date of Birth</label><input type="date" id="dob" required></div>
                    <div><label>Standard</label>
                        <select id="standard" required>
                            <option value="">Select Standard</option>
                            <option value="PreKG">PreKG</option>
                            <option value="LKG">LKG</option>
                            <option value="UKG">UKG</option>
                            <option value="1st Standard">1st Standard</option>
                            <option value="2nd Standard">2nd Standard</option>
                            <option value="3rd Standard">3rd Standard</option>
                            <option value="4th Standard">4th Standard</option>
                            <option value="5th Standard">5th Standard</option>
                            <option value="6th Standard">6th Standard</option>
                            <option value="7th Standard">7th Standard</option>
                            <option value="8th Standard">8th Standard</option>
                            <option value="9th Standard">9th Standard</option>
                            <option value="10th Standard">10th Standard</option>
                            <option value="11th Standard">11th Standard</option>
                            <option value="12th Standard">12th Standard</option>
                        </select>
                    </div>
                    <div><label>Parents / Guardians</label><input type="text" id="parents" placeholder="Enter parents name" required></div>
                    <div><label>Place</label><input type="text" id="place" placeholder="Enter place" required></div>
                    <div><label>State</label><select id="state" required><option value="">Select State</option></select></div>
                    <div><label>City (District)</label><select id="city" required><option value="">Select City</option></select></div>
                    <div><label>Age</label><select id="age" required><option value="">Select Age</option></select></div>
                </div>
                <button type="submit">➕ Add Record</button>
            </form>
        </div>

        <div class="record-card">
            <h2>Submitted Records</h2>
            <div id="recordsList"></div>
        </div>
    </div>
</div>

<script>
// -------------------- Login --------------------
function login() {
    const user = document.getElementById("username").value.trim();
    const pass = document.getElementById("password").value.trim();

    if (user === "star.international" && pass === "Star@1234") {
        sessionStorage.setItem("loggedIn", "true");
        showDashboard();
    } else {
        document.getElementById("error").style.display = "block";
    }
}

function showDashboard() {
    document.getElementById("loginBox").style.display = "none";
    document.getElementById("dashboard").style.display = "flex";
}

// -------------------- Logout --------------------
function logout() {
    sessionStorage.removeItem("loggedIn");
    document.getElementById("dashboard").style.display = "none";
    document.getElementById("loginBox").style.display = "block";
    document.getElementById("admissionForm").reset();
}

// -------------------- Check login --------------------
if(sessionStorage.getItem("loggedIn") === "true") {
    showDashboard();
}

// -------------------- Dashboard Logic --------------------
const statesAndDistricts = {
  "Andhra Pradesh": [
    "Alluri Sitharama Raju","Anakapalle","Anantapur","Annamayya","Bapatla","Chittoor","East Godavari","Eluru","Guntur",
    "Kadapa","Kakinada","Konaseema","Krishna","Kurnool","Nandyal","Nellore","Palnadu","Parvathipuram Manyam","Prakasam",
    "Srikakulam","Tirupati","Visakhapatnam","Vizianagaram","West Godavari"
  ],
  "Arunachal Pradesh": [
    "Anjaw","Changlang","Dibang Valley","East Kameng","East Siang","Itanagar Capital Complex","Kamle","Kra Daadi",
    "Kurung Kumey","Lepa Rada","Lohit","Longding","Lower Dibang Valley","Lower Siang","Lower Subansiri","Namsai",
    "Pakke Kessang","Papum Pare","Shi Yomi","Siang","Tawang","Tirap","Upper Dibang Valley","Upper Siang",
    "Upper Subansiri","West Kameng","West Siang"
  ],
  "Assam": [
    "Baksa","Barpeta","Biswanath","Bongaigaon","Cachar","Charaideo","Chirang","Darrang","Dhemaji","Dhubri","Dibrugarh",
    "Dima Hasao","Goalpara","Golaghat","Hailakandi","Hojai","Jorhat","Kamrup","Kamrup Metropolitan","Karbi Anglong",
    "Karimganj","Kokrajhar","Lakhimpur","Majuli","Morigaon","Nagaon","Nalbari","Sivasagar","Sonitpur",
    "South Salmara-Mankachar","Tinsukia","Udalguri","West Karbi Anglong"
  ],
  "Bihar": [
    "Araria","Arwal","Aurangabad","Banka","Begusarai","Bhagalpur","Bhojpur","Buxar","Darbhanga","East Champaran","Gaya",
    "Gopalganj","Jamui","Jehanabad","Kaimur","Katihar","Khagaria","Kishanganj","Lakhisarai","Madhepura","Madhubani",
    "Munger","Muzaffarpur","Nalanda","Nawada","Patna","Purnea","Rohtas","Saharsa","Samastipur","Saran","Sheikhpura",
    "Sheohar","Sitamarhi","Siwan","Supaul","Vaishali","West Champaran"
  ],
  "Chhattisgarh": [
    "Balod","Baloda Bazar","Balrampur","Bastar","Bemetara","Bijapur","Bilaspur","Dantewada","Dhamtari","Durg",
    "Gariaband","Gaurela-Pendra-Marwahi","Janjgir-Champa","Jashpur","Kabirdham","Kanker","Kondagaon","Korba","Koriya",
    "Mahasamund","Mungeli","Narayanpur","Raigarh","Raipur","Rajnandgaon","Sukma","Surajpur","Surguja"
  ],
  "Goa": ["North Goa","South Goa"],
  "Gujarat": [
    "Ahmedabad","Amreli","Anand","Aravalli","Banaskantha","Bharuch","Bhavnagar","Botad","Chhota Udepur","Dahod","Dang",
    "Devbhoomi Dwarka","Gandhinagar","Gir Somnath","Jamnagar","Junagadh","Kheda","Kutch","Mahisagar","Mehsana",
    "Morbi","Narmada","Navsari","Panchmahal","Patan","Porbandar","Rajkot","Sabarkantha","Surat","Surendranagar",
    "Tapi","Vadodara","Valsad"
  ],
  "Haryana": [
    "Ambala","Bhiwani","Charkhi Dadri","Faridabad","Fatehabad","Gurugram","Hisar","Jhajjar","Jind","Kaithal","Karnal",
    "Kurukshetra","Mahendragarh","Nuh","Palwal","Panchkula","Panipat","Rewari","Rohtak","Sirsa","Sonipat","Yamunanagar"
  ],
  "Himachal Pradesh": [
    "Bilaspur","Chamba","Hamirpur","Kangra","Kinnaur","Kullu","Lahaul and Spiti","Mandi","Shimla","Sirmaur","Solan",
    "Una"
  ],
  "Jharkhand": [
    "Bokaro","Chatra","Deoghar","Dhanbad","Dumka","East Singhbhum","Garhwa","Giridih","Godda","Gumla","Hazaribagh",
    "Jamtara","Khunti","Koderma","Latehar","Lohardaga","Pakur","Palamu","Ramgarh","Ranchi","Sahebganj",
    "Seraikela-Kharsawan","Simdega","West Singhbhum"
  ],
  "Karnataka": [
    "Bagalkote","Ballari","Belagavi","Bengaluru Rural","Bengaluru Urban","Bidar","Chamarajanagar","Chikballapur",
    "Chikkamagaluru","Chitradurga","Dakshina Kannada","Davanagere","Dharwad","Gadag","Hassan","Haveri","Kalaburagi",
    "Kodagu","Kolar","Koppal","Mandya","Mysuru","Raichur","Ramanagara","Shivamogga","Tumakuru","Udupi","Uttara Kannada",
    "Vijayapura","Yadgir"
  ],
  "Kerala": [
    "Alappuzha","Ernakulam","Idukki","Kannur","Kasaragod","Kollam","Kottayam","Kozhikode",
    "Malappuram","Palakkad","Pathanamthitta","Thiruvananthapuram","Thrissur","Wayanad"
  ],
  "Madhya Pradesh": [
    "Agar Malwa","Alirajpur","Anuppur","Ashoknagar","Balaghat","Barwani","Betul","Bhind","Bhopal","Burhanpur",
    "Chhatarpur","Chhindwara","Damoh","Datia","Dewas","Dhar","Dindori","Guna","Gwalior","Harda","Hoshangabad",
    "Indore","Jabalpur","Jhabua","Katni","Khandwa","Khargone","Mandla","Mandsaur","Morena","Narsinghpur",
    "Neemuch","Panna","Raisen","Rajgarh","Ratlam","Rewa","Sagar","Satna","Sehore","Seoni","Shahdol","Shajapur",
    "Sheopur","Shivpuri","Sidhi","Singrauli","Tikamgarh","Ujjain","Umaria","Vidisha"
  ],
  "Maharashtra": [
    "Ahmednagar","Akola","Amravati","Aurangabad","Beed","Bhandara","Buldhana","Chandrapur","Dhule","Gadchiroli",
    "Gondia","Hingoli","Jalgaon","Jalna","Kolhapur","Latur","Mumbai City","Mumbai Suburban","Nagpur","Nanded",
    "Nandurbar","Nashik","Osmanabad","Palghar","Parbhani","Pune","Raigad","Ratnagiri","Sangli","Satara","Sindhudurg",
    "Solapur","Thane","Wardha","Washim","Yavatmal"
  ],
  "Manipur": [
    "Bishnupur","Chandel","Churachandpur","Imphal East","Imphal West","Jiribam","Kakching","Kamjong",
    "Kangpokpi","Noney","Pherzawl","Senapati","Tamenglong","Tengnoupal","Thoubal","Ukhrul"
  ],
  "Meghalaya": [
    "East Garo Hills","East Jaintia Hills","East Khasi Hills","North Garo Hills","Ri Bhoi","South Garo Hills",
    "South West Garo Hills","South West Khasi Hills","West Garo Hills","West Jaintia Hills","West Khasi Hills"
  ],
  "Mizoram": [
    "Aizawl","Champhai","Hnahthial","Khawzawl","Kolasib","Lawngtlai","Lunglei","Mamit","Saiha","Saitual","Serchhip"
  ],
  "Nagaland": [
    "Chumoukedima","Dimapur","Kiphire","Kohima","Longleng","Mokokchung","Mon","Niuland","Peren",
    "Phek","Shamator","Tseminyu","Tuensang","Wokha","Zunheboto"
  ],
  "Odisha": [
    "Angul","Balangir","Balasore","Bargarh","Bhadrak","Boudh","Cuttack","Deogarh","Dhenkanal","Gajapati","Ganjam",
    "Jagatsinghpur","Jajpur","Jharsuguda","Kalahandi","Kandhamal","Kendrapara","Kendujhar","Khordha","Koraput",
    "Malkangiri","Mayurbhanj","Nabarangpur","Nayagarh","Nuapada","Puri","Rayagada","Sambalpur","Subarnapur","Sundargarh"
  ],
  "Punjab": [
    "Amritsar","Barnala","Bathinda","Faridkot","Fatehgarh Sahib","Fazilka","Ferozepur","Gurdaspur","Hoshiarpur",
    "Jalandhar","Kapurthala","Ludhiana","Mansa","Moga","Pathankot","Patiala","Rupnagar","SAS Nagar","Sangrur",
    "Shahid Bhagat Singh Nagar","Sri Muktsar Sahib","Tarn Taran"
  ],
  "Rajasthan": [
    "Ajmer","Alwar","Anupgarh","Balotra","Baran","Barmer","Bharatpur","Bhilwara","Bikaner","Bundi","Chittorgarh",
    "Churu","Dausa","Dholpur","Didwana-Kuchaman","Dungarpur","Ganganagar","Hanumangarh","Jaipur","Jaisalmer",
    "Jalore","Jhalawar","Jhunjhunu","Jodhpur","Karauli","Kota","Nagaur","Pali","Pratapgarh","Rajsamand",
    "Sawai Madhopur","Sikar","Sirohi","Tonk","Udaipur"
  ],
  "Sikkim": ["Gangtok","Gyalshing","Mangan","Namchi","Pakyong","Soreng"],
  "Tamil Nadu": [
    "Ariyalur","Chengalpattu","Chennai","Coimbatore","Cuddalore","Dharmapuri","Dindigul","Erode","Kallakurichi","Kanchipuram",
    "Kanyakumari","Karur","Krishnagiri","Madurai","Mayiladuthurai","Nagapattinam","Namakkal","Nilgiris","Perambalur",
    "Pudukkottai","Ramanathapuram","Ranipet","Salem","Sivaganga","Tenkasi","Thanjavur","Theni","Thoothukudi",
    "Tiruchirappalli","Tirunelveli","Tirupathur","Tiruppur","Tiruvallur","Tiruvannamalai","Tiruvarur","Vellore",
    "Viluppuram","Virudhunagar"
  ],
  "Telangana": [
    "Adilabad","Bhadradri Kothagudem","Hanumakonda","Hyderabad","Jagtial","Jangaon","Jayashankar Bhupalpally",
    "Jogulamba Gadwal","Kamareddy","Karimnagar","Khammam","Komaram Bheem","Mahabubabad","Mahabubnagar","Mancherial",
    "Medak","Medchal-Malkajgiri","Mulugu","Nagarkurnool","Nalgonda","Narayanpet","Nirmal","Nizamabad","Peddapalli",
    "Rajanna Sircilla","Rangareddy","Sangareddy","Siddipet","Suryapet","Vikarabad","Wanaparthy","Warangal","Yadadri Bhuvanagiri"
  ],
  "Tripura": ["Dhalai","Gomati","Khowai","North Tripura","Sepahijala","South Tripura","Unakoti","West Tripura"],
  "Uttar Pradesh": [
    "Agra","Aligarh","Ambedkar Nagar","Amethi","Amroha","Auraiya","Ayodhya","Azamgarh","Baghpat","Bahraich","Ballia",
    "Balrampur","Banda","Barabanki","Bareilly","Basti","Bhadohi","Bijnor","Budaun","Bulandshahr","Chandauli",
    "Chitrakoot","Deoria","Etah","Etawah","Faridabad","Farrukhabad","Fatehpur","Firozabad","Gautam Buddha Nagar",
    "Ghaziabad","Ghazipur","Gonda","Gorakhpur","Hamirpur","Hapur","Hardoi","Hathras","Jalaun","Jaunpur","Jhansi",
    "Kannauj","Kanpur Dehat","Kanpur Nagar","Kasganj","Kaushambi","Kheri","Kushinagar","Lalitpur","Lucknow",
    "Maharajganj","Mahoba","Mainpuri","Mathura","Mau","Meerut","Mirzapur","Moradabad","Muzaffarnagar",
    "Pilibhit","Pratapgarh","Prayagraj","Raebareli","Rampur","Saharanpur","Sambhal","Sant Kabir Nagar","Shahjahanpur",
    "Shamli","Shravasti","Siddharthnagar","Sitapur","Sonbhadra","Sultanpur","Unnao","Varanasi"
  ],
  "Uttarakhand": [
    "Almora","Bageshwar","Chamoli","Champawat","Dehradun","Haridwar","Nainital","Pauri Garhwal","Pithoragarh",
    "Rudraprayag","Tehri Garhwal","Udham Singh Nagar","Uttarkashi"
  ],
  "West Bengal": [
    "Alipurduar","Bankura","Birbhum","Cooch Behar","Dakshin Dinajpur","Darjeeling","Hooghly","Howrah","Jalpaiguri",
    "Jhargram","Kalimpong","Kolkata","Malda","Murshidabad","Nadia","North 24 Parganas","Paschim Bardhaman",
    "Paschim Medinipur","Purba Bardhaman","Purba Medinipur","Purulia","South 24 Parganas","Uttar Dinajpur"
  ],

  /* ===========================
     UNION TERRITORIES
  =========================== */
  "Andaman and Nicobar Islands": ["Nicobar","North and Middle Andaman","South Andaman"],
  "Chandigarh": ["Chandigarh"],
  "Dadra and Nagar Haveli and Daman and Diu": ["Dadra and Nagar Haveli","Daman","Diu"],
  "Delhi": [
    "Central Delhi","East Delhi","New Delhi","North Delhi","North East Delhi","North West Delhi",
    "Shahdara","South Delhi","South East Delhi","South West Delhi","West Delhi"
  ],
  "Jammu and Kashmir": [
    "Anantnag","Bandipora","Baramulla","Budgam","Doda","Ganderbal","Jammu","Kathua","Kishtwar","Kulgam",
    "Kupwara","Poonch","Pulwama","Rajouri","Ramban","Reasi","Samba","Shopian","Srinagar","Udhampur"
  ],
  "Ladakh": ["Kargil","Leh"],
  "Lakshadweep": ["Agatti","Amini","Andrott","Bitra","Chetlat","Kadmat","Kalpeni","Kavaratti","Kiltan","Minicoy"],
  "Puducherry": ["Karaikal","Mahe","Puducherry","Yanam"]
};

// Dropdowns
const stateSel = document.getElementById("state");
const citySel = document.getElementById("city");
const ageSel = document.getElementById("age");

// Populate states
Object.keys(statesAndDistricts).forEach(state => {
    stateSel.innerHTML += `<option value="${state}">${state}</option>`;
});

// Populate Age dropdown
for(let i=1;i<=100;i++){
    ageSel.innerHTML += `<option value="${i}">${i}</option>`;
}

// On state change populate cities
stateSel.addEventListener("change", () => {
    const districts = statesAndDistricts[stateSel.value] || [];
    citySel.innerHTML = "<option value=''>Select City</option>";
    districts.forEach(d => citySel.innerHTML += `<option value="${d}">${d}</option>`);
});

// -------------------- Local Storage and Records --------------------
let admissionCounter = parseInt(localStorage.getItem('lastAdmission')) || 10001;
document.getElementById("admissionNumber").value = admissionCounter;

const recordsList = document.getElementById("recordsList");
let savedRecords = JSON.parse(localStorage.getItem('admissionRecords')) || [];
renderRecords();

document.getElementById("admissionForm").addEventListener("submit", (e) => {
    e.preventDefault();

    const recordData = {
        admissionNo: admissionCounter,
        name: document.getElementById("name").value,
        dob: document.getElementById("dob").value,
        standard: document.getElementById("standard").value,
        parents: document.getElementById("parents").value,
        place: document.getElementById("place").value,
        state: stateSel.value,
        city: citySel.value,
        age: ageSel.value
    };

    savedRecords.push(recordData);
    localStorage.setItem('admissionRecords', JSON.stringify(savedRecords));

    admissionCounter++;
    localStorage.setItem('lastAdmission', admissionCounter);
    document.getElementById("admissionNumber").value = admissionCounter;

    // Reset only form fields except admission number
    document.getElementById("name").value = "";
    document.getElementById("dob").value = "";
    document.getElementById("standard").value = "";
    document.getElementById("parents").value = "";
    document.getElementById("place").value = "";
    stateSel.value = "";
    citySel.innerHTML = "<option value=''>Select City</option>";
    ageSel.value = "";

    renderRecords();
});

function renderRecords(){
    if(savedRecords.length === 0){
        recordsList.innerHTML = "<p>No records yet</p>";
        return;
    }

    let html = `<table>
        <thead>
        <tr>
            <th>Admission No</th>
            <th>Name</th>
            <th>DOB</th>
            <th>Standard</th>
            <th>Parents</th>
            <th>Place</th>
            <th>State</th>
            <th>City</th>
            <th>Age</th>
            <th>Action</th>
        </tr>
        </thead>
        <tbody>`;

    savedRecords.forEach((r,index) => {
        html += `<tr>
            <td>${r.admissionNo}</td>
            <td>${r.name}</td>
            <td>${r.dob}</td>
            <td>${r.standard}</td>
            <td>${r.parents}</td>
            <td>${r.place}</td>
            <td>${r.state}</td>
            <td>${r.city}</td>
            <td>${r.age}</td>
            <td><button onclick="deleteRecord(${index})">Delete</button></td>
        </tr>`;
    });

    html += "</tbody></table>";
    recordsList.innerHTML = html;
}

function deleteRecord(idx){
    savedRecords.splice(idx,1);
    localStorage.setItem('admissionRecords', JSON.stringify(savedRecords));

    let lastNo = savedRecords.length > 0 ? savedRecords[savedRecords.length -1].admissionNo + 1 : 10001;
    admissionCounter = lastNo;
    localStorage.setItem('lastAdmission', admissionCounter);
    document.getElementById("admissionNumber").value = admissionCounter;

    renderRecords();
}

// -------------------- Export to Excel --------------------
function exportToExcel() {
    if(savedRecords.length === 0) { alert("No records to export!"); return; }

    let csv = "Admission No,Name,DOB,Standard,Parents,Place,State,City,Age\n";
    savedRecords.forEach(r => {
        csv += `${r.admissionNo},${r.name},${r.dob},${r.standard},${r.parents},${r.place},${r.state},${r.city},${r.age}\n`;
    });

    const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = "admission_records.csv";
    link.click();
}
</script>

</body>
</html>
