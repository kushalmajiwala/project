<script>
    import { Nav, NavItem, NavLink, TabContent, TabPane } from "sveltestrap";
    let activeTab = 'Personal-Details';
</script>

<main>
    <div class="navbar">
    <TabContent pills class="tab-content responsive">
        <TabPane tabId="Personal-Details" tab="Personal-Details" class="fade show" active><span></span>
            <h2>Personal-Details</h2>
        </TabPane>
        <TabPane tabId="second" tab="Education-Details" class="fade show">
            <h2>Education-Details</h2>
        </TabPane>
        <TabPane tabId="third" tab="Skills" class="fade show">
            <h2>Skills</h2>
        </TabPane>
        <TabPane tabId="forth" tab="Interests" class="fade show">
            <h2>Interests</h2>
        </TabPane>
    </TabContent>
</div>
</main>
<style>
    .navbar {
        margin-top: -5%;
        margin-left: 1%;
        font-size: 20px;
    }
    @media screen and (max-width: 720px) {
       .navbar {
        display: inline-block;
       }
    }
</style>
