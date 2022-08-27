<script>
  import { Nav, NavItem, NavLink, TabContent, TabPane } from "sveltestrap";
  let personal = true;
  let education = false;
  let skill = false;
  let interest = false;
  
  let personalActive = true;
  let educationActive = false;
  let skillActive = false;
  let interestActive = false;

  function showPersonal() {
    //Showing the page of the content
    personal = true;
    education = false;
    skill = false;
    interest = false;

    //Showing the Active page tab
    personalActive = true;
    educationActive = false;
    skillActive = false;
    interestActive = false;
  }
  function showEducation() {
    personal = false;
    education = true;
    skill = false;
    interest = false;

    personalActive = false;
    educationActive = true;
    skillActive = false;
    interestActive = false;
  }
  function showSkill() {
    personal = false;
    education = false;
    skill = true;
    interest = false;

    personalActive = false;
    educationActive = false;
    skillActive = true;
    interestActive = false;
  }
  function showInterest() {
    personal = false;
    education = false;
    skill = false;
    interest = true;

    personalActive = false;
    educationActive = false;
    skillActive = false;
    interestActive = true;
  }
</script>

<main>
  <div class="navbar">
    <TabContent vertical pills>
      <div on:click={showPersonal}>
        <TabPane tabId="Personal-Details" tab="Personal-Details" active={personalActive} />
      </div>
      <div on:click={showEducation}>
        <TabPane tabId="education-Details" tab="education-Details" active={educationActive}/>
      </div>
      <div on:click={showSkill}>
        <TabPane tabId="skill" tab="skills" active={skillActive}/>
      </div>
      <div on:click={showInterest}>
        <TabPane tabId="interest" tab="interest" active={interestActive}/>
      </div>
    </TabContent>
  </div>
  <div class="all-content">
    {#if personal}
      <div class="personal-page my-content">
        <div class="page-header">
          <h1>Personal-page</h1>
        </div>
        <div class="pagination">
          <p class="next pagebtn" on:click={showEducation}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    {#if education}
      <div class="education-page my-content">
        <div class="page-header">
          <h1>Education-page</h1>
        </div>
        <div class="pagination">
          <p class="previous pagebtn" on:click={showPersonal}>&laquo; Previous</p>
          <p class="next pagebtn" on:click={showSkill}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    {#if skill}
      <div class="skill-page my-content">
        <div class="page-header">
          <h1>Skill-page</h1>
        </div>
        <div class="pagination">
          <p class="previous pagebtn" on:click={showEducation}>&laquo; Previous</p>
          <p class="next pagebtn" on:click={showInterest}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    {#if interest}
      <div class="interest-page my-content">
        <div class="page-header">
          <h1>Interest-page</h1>
        </div>
        <div class="pagination">
          <p class="previous pagebtn" on:click={showSkill}>&laquo; Previous</p>
        </div>
      </div>
    {/if}
  </div>
</main>

<style>
  .pagebtn {
    padding: 10px;
    background-color: rgb(70, 127, 212);
    cursor: pointer;
    border-radius: 15px;
    width: 120px;
    text-align: center;
    font-size: 20px;
    color: white;
    margin-top: 350px;
  }
  .pagebtn:hover {
    background-color: rgb(21, 80, 168);
  }
  .pagination {
    display: flex;
    justify-content: center;
    justify-content: space-around;
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
  }
  .my-content {
    background-color: white;
    height: 500px;
    width: 72%;
    margin-left: 2.2%;
    box-shadow: 2px 2px 5px 0px grey;
  }
  .all-content {
    margin-top: 2%;
  }
  @media screen and (min-width: 1350px) {
    .my-content {
      width: 76%;
    }
    .navbar {
      margin-top: -1%;
      padding-left: 33%;
    }
  }
  @media screen and (max-width: 1230px) {
    .navbar {
      padding-left: 41.5%;
      margin-top: 0%;
    }
    .my-content {
      width: 96%;
    }
  }
  @media screen and (max-width: 993px) {
    .navbar {
      padding-left: 35%;
    }
  }
  @media screen and (max-width: 800px) {
    .navbar {
      margin-top: -1%;
      padding-left: 37%;
    }
    .all-content {
      margin-top: 3%;
    }
  }
  @media screen and (max-width: 620px) {
    .navbar {
      padding-left: 30%;
      margin-top: -2%;
    }
  }
  @media screen and (max-width: 520px) {
    .navbar {
      padding-left: 28%;
      margin-top: -3.5%;
    }
  }
  @media screen and (max-width: 470px) {
    .navbar {
      padding-left: 26%;
      margin-top: -9%;
    }
  }
  @media screen and (max-width: 410px) {
    .navbar {
      padding-left: 24%;
      margin-top: -10%;
    }
  }
  @media screen and (max-width: 390px) {
    .navbar {
      padding-left: 22%;
      margin-top: -12%;
    }
  }
</style>
