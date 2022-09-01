<script>
  import {
    Nav,
    NavItem,
    NavLink,
    TabContent,
    TabPane,
    FormGroup,
    Input,
    Button,
    Modal,
    ModalBody,
    ModalFooter,
    ModalHeader,
Spinner
  } from "sveltestrap";
  import { createEventDispatcher } from "svelte";
  import axios from "axios";
  import { SupabaseStorageClient } from "@supabase/storage-js";
  import { onMount } from "svelte";

  const STORAGE_URL = "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1";
  const SERVICE_KEY =
    "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImR1aXlob21xd2t5c3Fzd2xraXB4Iiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTY1OTYxMjE0MywiZXhwIjoxOTc1MTg4MTQzfQ.AeJz_85Sbinf0ExIyk7V8cgQ8N_eQqOcyyRwf4BkDVU";

  const storageClient = new SupabaseStorageClient(STORAGE_URL, {
    apikey: SERVICE_KEY,
    Authorization: `Bearer ${SERVICE_KEY}`,
  });

  //Models Variables
  let open1 = false;
  let open2 = false;
  let open3 = false;
  let open4 = false;
  let open5 = false;

  //Modals Functions
  const toggle1 = () => (open1 = !open1);
  const toggle2 = () => (open2 = !open2);
  const toggle3 = () => (open3 = !open3);
  const toggle4 = () => (open4 = !open4);
  const toggle5 = () => (open5 = !open5);

  let progress = false;
  let uploading = false;
  let avatar;
  let imageurl = "";
  let files;
  let fileinput;
  let image_url = "";

  let personal = true;
  let education = false;
  let skill = false;
  let interest = false;
  let experience = false;

  let personalActive = true;
  let educationActive = false;
  let experienceActive = false;
  let skillActive = false;
  let interestActive = false;

  //Common Variables for all the pages
  let userid = "";
  let cvid = "";

  //Personal Page Variables
  let fname = "";
  let lname = "";
  let gender = "";
  let dob = "";
  let profession = "";
  let address = "";
  let personal_city = "";
  let personal_state = "";
  let phoneno = "";
  let email = "";
  let personal_pic_url = "";
  let cv_title = "";

  //Personal Page Borders
  let fname_border = "border: 1px solid #4c89ca;";
  let lname_border = "border: 1px solid #4c89ca;";
  let gender_border = "border: 1px solid #4c89ca;";
  let dob_border = "border: 1px solid #4c89ca;";
  let profession_border = "border: 1px solid #4c89ca;";
  let address_border = "border: 1px solid #4c89ca;";
  let personal_city_border = "border: 1px solid #4c89ca;";
  let personal_state_border = "border: 1px solid #4c89ca;";
  let phoneno_border = "border: 1px solid #4c89ca;";
  let email_border = "border: 1px solid #4c89ca;";
  let cv_title_border = "border: 1px solid #4c89ca;";

  //Education Page Borders
  let schoolname_border = "border: 1px solid #4c89ca;";
  let education_state_border = "border: 1px solid #4c89ca;";
  let education_city_border = "border: 1px solid #4c89ca;";
  let degree_border = "border: 1px solid #4c89ca;";
  let field_border = "border: 1px solid #4c89ca;";

  function personalValidate()
  {
    if(fname !== "" && lname !== "" && gender !== "" && dob !== "" && profession !== "" && address !== "" && personal_city !== "" && personal_state !== "" && phoneno !== "" && email !== "" && cv_title !== "")
    {
      fname_border = "border: 1px solid #4c89ca;";
      lname_border = "border: 1px solid #4c89ca;";
      gender_border = "border: 1px solid #4c89ca;";
      dob_border = "border: 1px solid #4c89ca;";
      profession_border = "border: 1px solid #4c89ca;";
      address_border = "border: 1px solid #4c89ca;";
      personal_city_border = "border: 1px solid #4c89ca;";
      personal_state_border = "border: 1px solid #4c89ca;";
      phoneno_border = "border: 1px solid #4c89ca;";
      email_border = "border: 1px solid #4c89ca;";
      cv_title_border = "border: 1px solid #4c89ca;";
    }
    if(fname == "" || lname == "" || gender == "" || dob == "" || profession == "" || address == "" || personal_city == "" || personal_state == "" || phoneno == "" ||email == "" || cv_title == "")
    {
      if(fname == "") fname_border = "border: 1px solid red;"; else fname_border = "border: 1px solid #4c89ca;";
      if(lname == "") lname_border = "border: 1px solid red;"; else lname_border = "border: 1px solid #4c89ca;";
      if(gender == "") gender_border = "border: 1px solid red;"; else gender_border = "border: 1px solid #4c89ca;";
      if(dob == "") dob_border = "border: 1px solid red;"; else dob_border = "border: 1px solid #4c89ca;";
      if(profession == "") profession_border = "border: 1px solid red;"; else profession_border = "border: 1px solid #4c89ca;";
      if(address == "") address_border = "border: 1px solid red;"; else address_border = "border: 1px solid #4c89ca;";
      if(personal_city == "") personal_city_border = "border: 1px solid red;"; else personal_city = "border: 1px solid #4c89ca;";
      if(personal_state == "") personal_state_border = "border: 1px solid red;"; else personal_state = "border: 1px solid #4c89ca;";
      if(phoneno == "") phoneno_border = "border: 1px solid red;"; else phoneno_border = "border: 1px solid #4c89ca;";
      if(email == "") email_border = "border: 1px solid red;"; else email_border = "border: 1px solid #4c89ca;";
      if(cv_title == "") cv_title_border = "border: 1px solid red;"; else cv_title = "border: 1px solid #4c89ca;";
      toggle1();
      showPersonal();
      return false
    }
    else
    {
      return true;
    }
  }

  //Education Page Variables
  let schoolname = "";
  let education_city = "";
  let education_state = "";
  let degree = "";
  let field = "";

  function educationValidate()
  {
    if (schoolname !== "" && education_city !== "" && education_state !== "" && degree !== "" && field !== "") {
      schoolname_border = "border: 1px solid #4c89ca;";
      education_state_border = "border: 1px solid #4c89ca;";
      education_city_border = "border: 1px solid #4c89ca;";
      degree_border = "border: 1px solid #4c89ca;";
      field_border = "border: 1px solid #4c89ca;";
    } 
    if (schoolname == "" || education_city == "" || education_state == "" || degree == "" || field == "") 
    {
      if(schoolname == "") schoolname_border = "border: 1px solid red"; else schoolname_border = "border: 1px solid #4c89ca;";
      if(education_city == "") education_city_border = "border: 1px solid red"; else education_city_border = "border: 1px solid #4c89ca;";
      if(education_state == "") education_state_border = "border: 1px solid red"; else education_state_border = "border: 1px solid #4c89ca;";
      if(degree == "") degree_border = "border: 1px solid red"; else degree_border = "border: 1px solid #4c89ca;"
      if(field == "") field_border = "border: 1px solid red"; else field_border = "border: 1px solid #4c89ca;";
      toggle2();
      showEducation();
      return false;
    } 
    return true;
  }

  //Experience Page Variables
  let job_title = "";
  let company_name = "";
  let experience_city = "";
  let experience_state = "";
  let experience_year = "";

  function experienceValidate()
  {
    
  }

  //Skill Page Variables
  let skill1 = "";
  let skill2 = "";
  let skill3 = "";
  let level1 = "";
  let level2 = "";
  let level3 = "";
  
  function skillValidate()
  {
    
  }

  //Interest Page variables
  let interest1 = "";
  let interest2 = "";
  let interest3 = "";

  function submitCV()
  {
    progress = true;
    if(personalValidate())
    {
      educationValidate();
    }
  }
  function progressStop()
  {
    progress = false;
  }

  const dispatch = createEventDispatcher();

  async function onFileSelected(e) {
    let image = e.target.files[0];
    let reader = new FileReader();
    reader.readAsDataURL(image);
    reader.onload = (e) => {
      avatar = e.target.result;
    };
    try {
      uploading = true;

      if (!files || files.length === 0) {
        throw new Error("You must select an image to upload.");
      }

      const file = files[0];
      const fileExt = file.name.split(".").pop();
      const fileName = `${Math.random()}.${fileExt}`;
      const filePath = `${fileName}`;

      const { data, error } = await storageClient
        .from("images")
        .upload(filePath, file);
      if (data) {
        image_url =
          "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/" +
          data.Key;
        personal_pic_url = image_url;
        console.log(image_url);
      }

      if (error) throw error;

      path = filePath;
      dispatch("upload");
    } catch (error) {
      alert(error.message);
    } finally {
      uploading = false;
    }
  }

  function startPoint()
  {
    userid = localStorage.getItem(username);
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/fetchimage/" +
                userid,
        };
        axios
            .request(options)
            .then(function (response) {
                imageurl = response.data.image;
            })
            .catch(function (error) {
                console.error(error);
            });

  }

  onMount(async () => {
        startPoint();
  });

  function showPersonal() {
    //Showing the page of the content
    personal = true;
    education = false;
    skill = false;
    interest = false;
    experience = false;

    //Showing the Active page tab
    personalActive = true;
    educationActive = false;
    skillActive = false;
    interestActive = false;
    experienceActive = false;
  }
  function showEducation() {
    personal = false;
    education = true;
    skill = false;
    interest = false;
    experience = false;

    personalActive = false;
    educationActive = true;
    skillActive = false;
    interestActive = false;
    experienceActive = false;
  }
  function showExperience() {
    personal = false;
    education = false;
    skill = false;
    interest = false;
    experience = true;

    personalActive = false;
    educationActive = false;
    skillActive = false;
    interestActive = false;
    experienceActive = true;
  }
  function showSkill() {
    personal = false;
    education = false;
    skill = true;
    interest = false;
    experience = false;

    personalActive = false;
    educationActive = false;
    skillActive = true;
    interestActive = false;
    experienceActive = false;
  }
  function showInterest() {
    personal = false;
    education = false;
    skill = false;
    interest = true;
    experience = false;

    personalActive = false;
    educationActive = false;
    skillActive = false;
    interestActive = true;
    experienceActive = false;
  }
  export let username;
  export let path;
</script>

<main>
  <div class="navbar">
    <TabContent vertical pills>
      <div on:click={showPersonal}>
        <TabPane
          tabId="Personal-Details"
          tab="Personal-Details"
          active={personalActive}
        />
      </div>
      <div on:click={showEducation}>
        <TabPane
          tabId="education-Details"
          tab="education-Details"
          active={educationActive}
        />
      </div>
      <div on:click={showExperience}>
        <TabPane
          tabId="experience-Details"
          tab="experience-Details"
          active={experienceActive}
        />
      </div>
      <div on:click={showSkill}>
        <TabPane tabId="skill" tab="skills" active={skillActive} />
      </div>
      <div on:click={showInterest}>
        <TabPane tabId="interest" tab="interest" active={interestActive} />
      </div>
    </TabContent>
  </div>
  <div class="all-content">
    <!-- Personal Page -->
    {#if personal}
      <div class="personal-page my-content">
        <div class="page-header">
          <h1>Personal-page</h1>
        </div>
        <div class="form-row form-content" style="margin-top: 5%;">
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter First Name">
              <Input placeholder="Enter First Name" style={fname_border} bind:value={fname}/>
            </FormGroup>
          </div>
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Last Name">
              <Input placeholder="Enter Last Name" style={lname_border} bind:value={lname}/>
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content">
          <div class="form-group col-md-4">
            <FormGroup floating label="Select Gender">
              <Input
                type="select"
                name="select"
                id="exampleSelect"
                style={gender_border}
                bind:value={gender}
              >
                <option style="cursor:pointer;">Male</option>
                <option style="cursor:pointer;">Female</option>
                <option style="cursor:pointer;">Other</option>
              </Input>
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Select Date of Birth">
              <Input
                style={dob_border}
                type="date"
                name="date"
                id="exampleDate"
                placeholder="date placeholder"
                bind:value={dob}
              />
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter Profession">
              <Input placeholder="Enter Profession" style={profession_border} bind:value={profession}/>
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content">
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter Address">
              <Input placeholder="Enter Address" style={address_border} bind:value={address}/>
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter City">
              <Input placeholder="Enter City" style={personal_city_border} bind:value={personal_city}/>
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter State">
              <Input placeholder="Enter State" style={personal_state_border} bind:value={personal_state}/>
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content">
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Phone Number">
              <Input placeholder="Enter Phone Number" style={phoneno_border} bind:value={phoneno}/>
            </FormGroup>
          </div>
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Email">
              <Input placeholder="Enter Email" style={email_border} bind:value={email}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter CV Title">
              <Input placeholder="Enter CV Title" style={cv_title_border} bind:value={cv_title}/>
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content image-form">
          <div class="image-container" style="margin-bottom: 2%;">
            <div id="app">
              <h3 style="margin-bottom: 7%;">Upload Image For CV</h3>
              {#if avatar}
                <img class="avatar" src={avatar} alt="d" />
              {:else }
                <img
                  class="avatar"
                  src="https://cdn4.iconfinder.com/data/icons/small-n-flat/24/user-alt-512.png"
                  alt=""
                />
              {/if}

              <label class="button primary block uploadbtn" for="single"
                ><i class="bi bi-upload" />
                {uploading ? "Uploading ..." : "Upload"}
              </label>
            </div>
            <input
              style="display:none"
              type="file"
              id="single"
              accept="image/*"
              bind:files
              bind:this={fileinput}
              on:change={(e) => onFileSelected(e)}
              disabled={uploading}
            />
          </div>
        </div>
        <div class="btncontainer">
          <p class="next pagebtn" on:click={showEducation}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    <!-- Education Page -->
    {#if education}
      <div class="education-page my-content">
        <div class="page-header">
          <h1>Education-page</h1>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%; margin-top: 5%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter School Name">
              <Input placeholder="Enter School Name" style={schoolname_border} bind:value={schoolname}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter City">
              <Input placeholder="Enter City" style={education_city_border} bind:value={education_city}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter State">
              <Input placeholder="Enter State" style={education_state_border} bind:value={education_state}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Degree">
              <Input placeholder="Enter Degree" style={degree_border} bind:value={degree}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%; margin-bottom: 3%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Field">
              <Input placeholder="Enter Field" style={field_border}  bind:value={field}/>
            </FormGroup>
          </div>
        </div>
        <div class="btncontainer">
          <p class="previous pagebtn" on:click={showPersonal}>
            &laquo; Previous
          </p>
          <p class="next pagebtn" on:click={showExperience}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    <!-- Experience Page -->
    {#if experience}
      <div class="education-page my-content">
        <div class="page-header">
          <h1>Experience-page</h1>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%; margin-top: 5%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Job Title">
              <Input placeholder="Enter Job Title" style="border: 1px solid #4c89ca;" bind:value={job_title}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Company Name">
              <Input placeholder="Enter Company Name" style="border: 1px solid #4c89ca;" bind:value={company_name}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter City">
              <Input placeholder="Enter City" style="border: 1px solid #4c89ca;" bind:value={experience_city}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter State">
              <Input placeholder="Enter State" style="border: 1px solid #4c89ca;" bind:value={experience_state}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%; margin-bottom: 3%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Experience(Years)">
              <Input placeholder="Enter Experience" style="border: 1px solid #4c89ca;" bind:value={experience_year}/>
            </FormGroup>
          </div>
        </div>
        <div class="btncontainer">
          <p class="previous pagebtn" on:click={showEducation}>
            &laquo; Previous
          </p>
          <p class="next pagebtn" on:click={showSkill}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    <!-- Skill Page -->
    {#if skill}
      <div class="skill-page my-content">
        <div class="page-header">
          <h1>Skill-page</h1>
        </div>
        <div class="form-row form-content" style="margin-top: 5%;">
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Skill">
              <Input placeholder="Enter Skill" style="border: 1px solid #4c89ca;" bind:value={skill1}/>
            </FormGroup>
          </div>
          <div class="form-group col-md-6">
            <FormGroup floating label="Select Level">
              <Input
                type="select"
                name="select"
                id="exampleSelect"
                style="cursor:pointer; border: 1px solid #4c89ca;"
                bind:value={level1}
              >
                <option style="cursor:pointer;">Beginner</option>
                <option style="cursor:pointer;">Intermediate</option>
                <option style="cursor:pointer;">Expert</option>
              </Input>
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content">
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Skill">
              <Input placeholder="Enter Skill" style="border: 1px solid #4c89ca;" bind:value={skill2}/>
            </FormGroup>
          </div>
          <div class="form-group col-md-6">
            <FormGroup floating label="Select Level">
              <Input
                type="select"
                name="select"
                id="exampleSelect"
                style="cursor:pointer; border: 1px solid #4c89ca;"
                bind:value={level2}
              >
                <option style="cursor:pointer;">Beginner</option>
                <option style="cursor:pointer;">Intermediate</option>
                <option style="cursor:pointer;">Expert</option>
              </Input>
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content" style="margin-bottom: 5%;">
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Skill">
              <Input placeholder="Enter Skill" style="border: 1px solid #4c89ca;" bind:value={skill3}/>
            </FormGroup>
          </div>
          <div class="form-group col-md-6">
            <FormGroup floating label="Select Level">
              <Input
                type="select"
                name="select"
                id="exampleSelect"
                style="cursor:pointer; border: 1px solid #4c89ca;"
                bind:value={level3}
              >
                <option style="cursor:pointer;">Beginner</option>
                <option style="cursor:pointer;">Intermediate</option>
                <option style="cursor:pointer;">Expert</option>
              </Input>
            </FormGroup>
          </div>
        </div>
        <div class="btncontainer">
          <p class="previous pagebtn" on:click={showExperience}>
            &laquo; Previous
          </p>
          <p class="next pagebtn" on:click={showInterest}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    <!-- Interest Page -->
    {#if interest}
      <div class="interest-page my-content">
        <div class="page-header">
          <h1>Interest-page</h1>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%; margin-top: 5%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Interest">
              <Input placeholder="Enter Interest" style="border: 1px solid #4c89ca;" bind:value={interest1}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Interest">
              <Input placeholder="Enter Interest" style="border: 1px solid #4c89ca;" bind:value={interest2}/>
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%; margin-bottom: 5%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter Interest">
              <Input placeholder="Enter Interest" style="border: 1px solid #4c89ca;" bind:value={interest3}/>
            </FormGroup>
          </div>
        </div>
        <div class="btncontainer">
          <p class="previous pagebtn" on:click={showSkill}>&laquo; Previous</p>
          <Button color="success" on:click={submitCV} style="width: 120px; height: 50px; border-radius: 15px; font-size: 22px; margin-top: 25px;">
            {#if progress}
              <p><Spinner size="sm"/> Saving</p>
            {:else}
              <p><i class="bi bi-file-earmark-check-fill"></i>  Save </p>
            {/if}
          </Button>
        </div>
      </div>
    {/if}
  </div>
  <div class="Modals">
     <!-- empty-modal -->
     <Modal header="Message" isOpen={open1}>
      <ModalBody>Fields Cannot be empty on Personal Details Page...</ModalBody>
      <ModalFooter>
        <Button color="danger" class="float-right" on:click={toggle1} on:click={progressStop}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
     <!-- empty-modal -->
     <Modal header="Message" isOpen={open2}>
      <ModalBody>Fields Cannot be empty on Education Details Page...</ModalBody>
      <ModalFooter>
        <Button color="danger" class="float-right" on:click={toggle2} on:click={progressStop}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
     <!-- empty-modal -->
     <Modal header="Message" isOpen={open3}>
      <ModalBody>Fields Cannot be empty on Experience Details Page...</ModalBody>
      <ModalFooter>
        <Button color="danger" class="float-right" on:click={toggle3} on:click={progressStop}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
     <!-- empty-modal -->
     <Modal header="Message" isOpen={open4}>
      <ModalBody>Fields Cannot be empty on Skill Details Page...</ModalBody>
      <ModalFooter>
        <Button color="danger" class="float-right" on:click={toggle4} on:click={progressStop}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
    <!-- empty-modal -->
    <Modal header="Message" isOpen={open5}>
      <ModalBody>Fields Cannot be empty on Interest Details Page...</ModalBody>
      <ModalFooter>
        <Button color="danger" class="float-right" on:click={toggle5} on:click={progressStop}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
  </div>
</main>

<style>
  .avatar {
        display: flex;
        height: 150px;
        width: 150px;
        margin-top: 2%;
        margin-left: 25%;
        box-shadow: 0px 0px 2px 0px grey;
    }
  .uploadbtn {
        cursor: pointer;
        color: white;
        background-color: #007bff;
        filter: blur(2%);
        height: 38px;
        width: 120px;
        line-height: 38px;
        text-align: center;
        border-radius: 10px;
        margin-top: 3%;
        border: 1px solid #0772e3;
        filter: saturate(200%);
    }
    .uploadbtn:hover {
        background-color: #0772e3;
    }
  .image-container {
      height: 290px;
      width: 300px;
      background-color: white;
      margin-top: 0%;
      margin-left: 31%;
      box-shadow: 0px 0px 7px 0px grey;
      border-radius: 1%;
      margin-bottom: 4%;
      text-align: center;
  }
  .form-content {
    width: 90%;
    margin-left: 5%;
  }
  .btncontainer {
    width: 100%;
    display: flex;
    justify-content: space-around;
    /* padding-left: 43%; */
  }
  .pagebtn {
    padding: 10px;
    background-color: rgb(70, 127, 212);
    cursor: pointer;
    border-radius: 15px;
    width: 120px;
    text-align: center;
    font-size: 20px;
    color: white;
    margin-top: 25px;
  }
  .pagebtn:hover {
    background-color: rgb(21, 80, 168);
  }
  .navbar {
    margin-top: -4%;
    padding: 15px;
    font-size: 20px;
    text-align: center;
    background-color: white;
    width: 100%;
    opacity: 0.8;
    font-size: 25px;
    padding-left: 30%;
    box-shadow: 2px 2px 5px 0px grey;
  }
  .page-header {
    width: 100%;
    text-align: center;
    border-bottom: 1px solid rgb(179, 176, 214);
    padding: 10px;
    margin-bottom: 2%;
  }
  .my-content {
    background-color: white;
    height: 950px;
    width: 74%;
    margin-left: 2%;
    box-shadow: 2px 2px 5px 0px grey;
  }
  .all-content {
    margin-top: 2%;
    width: 100%;
    text-align: center;
  }
  @media screen and (min-width: 1350px) {
    .my-content {
      width: 76%;
      margin-left: 3%;
    }
    .navbar {
      margin-top: -2.5%;
      padding-left: 33%;
    }
    .image-container {
      margin-left: 36%;
    }
  }
  @media screen and (max-width: 1230px) {
    .navbar {
      padding-left: 41.5%;
      margin-top: -3%;
    }
    .my-content {
      width: 90%;
      margin-left: 4.8%;
    }
    .image-container {
      margin-left: 35%;
    }
  }
  @media screen and (max-width: 993px) {
    .navbar {
      padding-left: 35%;
      margin-top: -5%;
    }
    .image-container {
      margin-left: 33%;
    }
  }
  @media screen and (max-width: 800px) {
    .navbar {
      margin-top: -5%;
      padding-left: 37%;
    }
    .all-content {
      margin-top: 3%;
    }
    .image-container {
      margin-left: 30%;
    }
  }
  @media screen and (max-width: 770px) {
   .my-content {
    height: 1450px;
   }
   .image-container {
      margin-left: 26%;
    }
  }
  @media screen and (max-width: 670px) {
    .image-container {
      margin-left: 23%;
    }
  }
  @media screen and (max-width: 620px) {
    .navbar {
      padding-left: 30%;
      margin-top: -5%;
    }
    .image-container {
      margin-left: 20%;
    }
  }
  @media screen and (max-width: 570px) {
    .image-container {
      margin-left: 18%;
    }
  }
  @media screen and (max-width: 540px) {
    .image-container {
      margin-left: 15%;
    }
    .page-header {
      margin-bottom: 9%;
    }
    .form-content {
      margin-top: -2%;
    }
  }
  @media screen and (max-width: 520px) {
    .navbar {
      padding-left: 28%;
      margin-top: -7%;
    }
    .image-container {
      margin-left: 13%;
    }
  }
  @media screen and (max-width: 470px) {
    .navbar {
      padding-left: 26%;
      margin-top: -9%;
    }
    .image-container {
      margin-left: 11.5%;
    }
  }
  @media screen and (max-width: 450px) {
    .image-container {
      margin-left: 9.5%;
    }
  }
  @media screen and (max-width: 430px) {
    .image-container {
      margin-left: 7%;
    }
  }
  @media screen and (max-width: 410px) {
    .navbar {
      padding-left: 24%;
      margin-top: -7%;
    }
    .image-container {
      margin-left: 5%;
    }
  }
  @media screen and (max-width: 390px) {
    .navbar {
      padding-left: 22%;
      margin-top: -12%;
    }
    .image-container {
      margin-left: 1%;
    }
  }
  @media screen and (max-width: 360px) {
    .navbar {
      padding-left: 19.5%;
      margin-top: -13%;
    }
  }
  @media screen and (max-width: 350px) {
    .navbar {
      padding-left: 20%;
      margin-top: -14%;
    }
  }
  @media screen and (max-width: 330px) {
    .navbar {
      padding-left: 18%;
      margin-top: -15%;
    }
  }
</style>
