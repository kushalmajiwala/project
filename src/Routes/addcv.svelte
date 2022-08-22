<script>
    import { Nav, NavItem, NavLink, TabContent, TabPane } from "sveltestrap";
    let personal = true;
    let education = false;
    let skill = false;
    let interest = false;

    function showPersonal() {
        personal = true;
        education = false;
        skill = false;
        interest = false;
    }
    function showEducation() {
        personal = false;
        education = true;
        skill = false;
        interest = false;
    }
    function showSkill() {
        personal = false;
        education = false;
        skill = true;
        interest = false;
    }
    function showInterest() {
        personal = false;
        education = false;
        skill = false;
        interest = true;
    }
</script>

<main>
    <div class="navbar">
        <TabContent vertical pills>
            <div on:click={showPersonal}>
                <TabPane
                    tabId="Personal-Details"
                    tab="Personal-Details"
                    active
                />
            </div>
            <div on:click={showEducation}>
                <TabPane
                    tabId="education-Details"
                    tab="education-Details"
                />
            </div>
            <div on:click={showSkill}>
                <TabPane
                    tabId="skill"
                    tab="skills"
                />
            </div>
            <div on:click={showInterest}>
                <TabPane
                    tabId="interest"
                    tab="interest"
                />
            </div>
        </TabContent>
    </div>
    <div class="all-content">
        {#if personal}
            <div class="personal-page my-content">
                <div class="page-header">
                    <h1>Personal-page</h1>
                </div>
            </div>
        {/if}
        {#if education}
            <div class="education-page my-content">
                <div class="page-header">
                    <h1>Education-page</h1>
                </div>
            </div>
        {/if}
        {#if skill}
            <div class="skill-page my-content">
                <div class="page-header">
                    <h1>Skill-page</h1>
                </div>
            </div>
        {/if}
        {#if interest}
            <div class="interest-page my-content">
                <div class="page-header">
                    <h1>Interest-page</h1>
                </div>
            </div>
        {/if}
    </div>
</main>

<style>
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
        margin-top: -3%;
      }
    }
    @media screen and (max-width: 1230px) {
      .navbar {
        padding-left: 40%;
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
    @media screen and (max-width:800px) {
      .navbar {
        padding-left: 32%;
      }
      .all-content {
        margin-top: 3%;
      }
      .navbar {
        margin-top: -5%;
      }
    }
    @media screen and (max-width: 620px) {
      .navbar {
        padding-left: 30%;
      }
      .navbar {
        margin-top: -7%;
      }
    }
    @media screen and (max-width: 520px) {
      .navbar {
        padding-left: 28%;
      }
      .navbar {
        margin-top: -10%;
      }
    }
    @media screen and (max-width: 470px) {
      .navbar {
        padding-left: 26%;
      }
      .navbar {
        margin-top: -15%;
      }
    }
    @media screen and (max-width: 410px) {
      .navbar {
        padding-left: 24%;
      }
    }
    @media screen and (max-width: 390px) {
      .navbar {
        padding-left: 22%;
      }
    }
</style>
