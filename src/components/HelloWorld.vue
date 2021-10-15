<template>
  <div id="app2">
    <form >
    <div v-if="pass_right === 'true'" class="topnav">
      <label id="userid" class="active" title="" @mouseenter="hov()">User: {{user}}</label>
      <label class="ab">Department: {{departnment}}</label>
    </div ><br>
    <div v-if="pass_right === 'true'">
      <button class="tablink" @click="openPage('btn1','Codes','#b94242;')" id="btn1">Codes</button>
      <button class="tablink" @click="openPage('btn2','Addcode','#b94242')" id="btn2">Add code</button>
      <button class="tablink" type = "button" onClick ="window.location.href='https://clock-system-6a6f8.web.app/#/';" id="btn3">Clocking system</button>
      <div id="Codes" class="tabcontent">
        <label v-for="n in linksGitSize" :key= "n">
          <a id="t" title="t" href= "" @mouseenter = "gitsadd(n)">{{linksGit[n-1].programs_}}</a><br>
        </label>
      </div>
      <div id="Addcode" class="tabcontent">
        <label>Still under construction</label>
      </div>
    </div>
<div v-if="pass_right === 'false'" id="app">
  <label>User name:   </label>
  <input type= "text" v-model="user" placeholder="Enter user name" size = "25">
  <br><br>
  <label>Password:    </label>
  <input type= "password" v-model="pass" placeholder="Enter password" autocomplete="off" size = "25">
  <br><br>
  <button @click= "checkpassword(1); getgithublinks();" type = "button">
       Log in
  </button><br><br>
</div>

     </form>
  </div>
</template>
<script>
import MD5 from '../../node_modules/md5'
export default {
  data () {
    return {
      runCount: 0,
      linksGit: [],
      linksGitSize: 0,
      departnment: 'empty',
      gtemail: 0,
      searchemail: '@eafricatelecoms.co.za',
      z: 0,
      showtable: 'false',
      is_manager: '',
      newPass: '',
      newPass_con: '',
      is_superior: 'no',
      newEmployee: '',
      newEmail: '',
      date_: 'n',
      newuser: 'false',
      user: '',
      pass: '',
      cpass: '',
      n: '',
      pass_right: 'false',
      lim: 4,
      username: '',
      email: '@eafricatelecoms.co.za',
      resultsFetched_em: '',
      resultsFetched_2: '',
      resultsFetched_3: '',
      resultsFetched: '', // Tis variable will store the results fetched
      /* all: '',
      all_email: '',
      all_in: '',
      all_out: '',
      all_day: '',
      space: '           ', */
      i: 0,
      all_nam: [],
      all_email: [],
      all_in: [],
      all_out: [],
      all_date: [],
      all_pass: [],
      all_man: [],
      employeesTable: 'false',
      em_nam: [],
      em_email: [],
      em_superior: [],
      em_manager: [],
      em_hours: [],
      number_of_employees: 0,
      x: 0,
      count: 0,
      c_2: 0,
      newp: 'false',
      cPass: '',
      cPass_con: '',
      lock: 'false',
      offtime: '17:00:00'
    }
  },
  methods: {
    async getgithublinks () {
      await fetch(`https://warm-springs-22910.herokuapp.com/fn_get_gitlink/`)
        .then(response => response.json())
        .then(results => (this.linksGit = results))
      this.linksGitSize = this.linksGit.length
      console.log('links opened' + this.linksGit.length)
    },
    openPage (btn, pageName, color) {
      // Hide all elements with class="tabcontent" by default */
      var i, tabcontent, tablinks
      tabcontent = document.getElementsByClassName('tabcontent')
      for (i = 0; i < tabcontent.length; i++) {
        tabcontent[i].style.display = 'none'
      }
      // Remove the background color of all tablinks/buttons
      tablinks = document.getElementsByClassName('tablink')
      for (i = 0; i < tablinks.length; i++) {
        tablinks[i].style.backgroundColor = ''
      }
      // Show the specific tab content
      document.getElementById(pageName).style.display = 'block'
      // Add the specific color to the button used to open the tab content
      document.getElementById(pageName).style.backgroundColor = '#b94242'
      document.getElementById(btn).style.backgroundColor = '#b94242'
    },
    hov () {
      document.getElementById('userid').title = this.email
    },
    gitsadd (n) {
      if (this.runCount === 0) {
        for (let index = 0; index < this.linksGitSize; index++) {
          document.getElementById('t').title = this.linksGit[index].programs_
          document.getElementById('t').id = this.linksGit[index].programs_
          document.getElementById(this.linksGit[index].programs_).href = this.linksGit[n - 1].links_
          document.getElementById(this.linksGit[index].programs_).style.color = 'white'
        }
      }
      this.runCount++
    },
    async viewemployees (i) {
      await fetch(`https://warm-springs-22910.herokuapp.com/getall_workers`)
        .then(response => response.json())
        .then(results => (this.resultsFetched_3 = results))
      if (i === 1) {
        this.employeesTable = 'true'
        this.number_of_employees = this.resultsFetched_3.length
        for (this.count = 0; this.count < this.number_of_employees; this.count++) {
          this.em_nam[this.count] = this.resultsFetched_3[this.count].name_
          this.em_email[this.count] = this.resultsFetched_3[this.count].email_
          this.em_manager[this.count] = this.resultsFetched_3[this.count].manager_
          this.em_hours[this.count] = this.resultsFetched_3[this.count].hours_
          // console.log('hours for'+ this.em_nam[this.count] + ' are :' + this.em_hours[this.count])
        }
      }
    },
    issuperior_emp () {
      const supercherckbox = document.getElementById('is_superior')
      // console.log('check box: ' + supercherckbox.checked)
      if (supercherckbox.checked) {
        this.is_superior = 'yes'
      } else {
        this.is_superior = 'no'
      }
      // console.log(this.is_superior)
    },
    async load (i) {
      if (this.date_ !== 'n') {
        this.z++
        await fetch(`https://warm-springs-22910.herokuapp.com/bydate/${this.date_}`)
          .then(response => response.json())
          .then(results => (this.resultsFetched = results))
        if (i === 1) {
          this.showtable = 'true'
          // console.log(this.resultsFetched)
          this.lim = this.resultsFetched.length
          for (this.i = 0; this.i < this.resultsFetched.length; this.i++) {
            this.all_nam[this.i] = this.resultsFetched[this.i].name_
            this.all_email[this.i] = this.resultsFetched[this.i].email_
            this.all_in[this.i] = this.resultsFetched[this.i].log_in_
            if (this.resultsFetched[this.i].log_out_ !== '00:00:00') {
              this.all_out[this.i] = this.resultsFetched[this.i].log_out_
            } else {
              this.all_out[this.i] = 'Not out yet'
            }
            this.all_date[this.i] = this.resultsFetched[this.i].date_
            this.all_pass[this.i] = this.resultsFetched[this.i].password_
          }
        }
      }
    },
    test () {
      this.newuser = 'true'
      // console.log(this.newuser)
    },
    async checkpassword (i) {
      // console.log('Password: ' + MD5('Tebogompete#3').toString())
      this.x++
      await fetch(`https://warm-springs-22910.herokuapp.com/getall_workers`)
        .then(response => response.json())
        .then(results => (this.resultsFetched_2 = results))
      this.cpass = MD5(this.pass).toString()
      for (this.i = 0; this.i < this.resultsFetched_2.length; this.i++) {
        if (this.user === this.resultsFetched_2[this.i].name_) {
          if (this.cpass === this.resultsFetched_2[this.i].password_ && i === 1) {
            this.pass_right = 'true'
            // console.log('      pass right     ')
            this.email = this.resultsFetched_2[this.i].email_
            this.departnment = this.resultsFetched_2[this.i].department_
            if (this.resultsFetched_2[this.i].manager_) {
              this.is_manager = 'true'
            } else {
              this.is_manager = 'false'
            }
          }
        }
      }
      if (this.pass_right !== 'true' && this.x > 1 && i === 1) {
        alert('incorrect login details')
        this.x = 0
      }
    },
    async fetchRes (i) {
      this.z++
      this.all = ''
      if (this.is_manager === 'true') {
        await fetch('https://warm-springs-22910.herokuapp.com/all')
          .then(response => response.json())// This will convert it to a more readable way
          .then(results => (this.resultsFetched = results))
        if (i === 1) {
          this.lim = this.resultsFetched.length
          for (this.i = 0; this.i < this.resultsFetched.length; this.i++) {
            this.all_nam[this.i] = this.resultsFetched[this.i].name_
            this.all_email[this.i] = this.resultsFetched[this.i].email_
            this.all_in[this.i] = this.resultsFetched[this.i].log_in_
            if (this.resultsFetched[this.i].log_out_ !== '00:00:00') {
              this.all_out[this.i] = this.resultsFetched[this.i].log_out_
            } else {
              this.all_out[this.i] = 'Not out yet'
            }
            this.all_date[this.i] = this.resultsFetched[this.i].date_
            this.all_pass[this.i] = this.resultsFetched[this.i].password_
          }
          this.showtable = 'true'
        }
      } else {
        await fetch(`https://warm-springs-22910.herokuapp.com/get_by_email/${this.email}`)
          .then(response => response.json())// This will convert it to a more readable way
          .then(results => (this.resultsFetched = results))
        this.lim = this.resultsFetched.length
        if (i === 1) {
          for (this.i = 0; this.i < this.resultsFetched.length; this.i++) {
            this.all_nam[this.i] = this.resultsFetched[this.i].name_
            this.all_email[this.i] = this.email
            this.all_in[this.i] = this.resultsFetched[this.i].log_in_
            if (this.resultsFetched[this.i].log_out_ !== '00:00:00') {
              this.all_out[this.i] = this.resultsFetched[this.i].log_out_
            } else {
              this.all_out[this.i] = 'Not out yet'
            }
            this.all_date[this.i] = this.resultsFetched[this.i].date_
            this.all_pass[this.i] = this.resultsFetched[this.i].password_
          }
          this.showtable = 'true'
        }
      }
      //   console.log('Password: ' + MD5('Tebogompete#3').toString())
    },
    async getbyemail (i) {
      if (this.searchemail !== '@eafricatelecoms.co.za' && this.searchemail !== '') {
        this.gtemail++
        await fetch(`https://warm-springs-22910.herokuapp.com/get_by_email/${this.searchemail}`)
          .then(response => response.json())
          .then(results => (this.resultsFetched_em = results))
        // console.log(this.resultsFetched_em)
        if (this.gtemail > 1 && i === 1) {
          this.showtable = 'true'
          this.lim = this.resultsFetched_em.length
          for (this.i = 0; this.i < this.resultsFetched_em.length; this.i++) {
            this.all_nam[this.i] = this.resultsFetched_em[this.i].name_
            this.all_email[this.i] = this.searchemail
            this.all_in[this.i] = this.resultsFetched_em[this.i].log_in_
            if (this.resultsFetched_em[this.i].log_out_ !== '00:00:00') {
              this.all_out[this.i] = this.resultsFetched_em[this.i].log_out_
            } else {
              this.all_out[this.i] = 'Not out yet'
            }
            this.all_date[this.i] = this.resultsFetched_em[this.i].date_
            this.all_pass[this.i] = this.resultsFetched_em[this.i].password_
          }
          this.gtemail = 0
        }
      }
    }
  }
}
</script>

<style scoped>

</style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body, html {
  height: 100%;
  margin: 0;
  font-family: Arial;
}

/* Style tab links */
.tablink {
  background-color: #555;
  color: white;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 16px;
  font-size: 17px;
  width: 25%;
}

.tablink:hover {
  background-color: #777;
}

/* Style the tab content (and add height:100% for full page content) */
.tabcontent {
  color: white;
  display: none;
  padding: 100px 20px;
  height: 100%;
}

#Home {background-color: red;}
#News {background-color: green;}
#Contact {background-color: blue;}
#About {background-color: orange;}
.topnav {
  overflow: hidden;
  background-color: #b94242;
}
.topnav label {
  float: left;
  color: #ffffff;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
  font-size: 17px;
}
.topnav label.ab {
  float: right;
  color: #ffffff;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
  font-size: 17px;
}
#atl{
  font-weight: normal;
  text-align: right;
}
h1{
  font-weight: normal;
  text-align: right;
}
h2{
  font-weight: normal;
  text-align: left;
  font-size: 90%;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

label {
   font-weight: normal;
  text-align: right;
}
a{
    color: #ffffff;
}

user agent stylesheet
a:-webkit-any-link {
    color: -webkit-link;
    cursor: pointer;
    text-decoration: underline;
}
ul li {
    line-height: 24px;
}
user agent stylesheet
li {
    text-align: -webkit-match-parent;
}

#js-mainnav.megamenu ul {
    list-style: outside none none;
}
user agent stylesheet
ul {
    list-style-type: disc;
}

#js-mainnav.megamenu .js-megamenu {
    font-size: 12px;
    margin: 0px;
    padding: 0px;
}
#js-mainnav.megamenu {
    line-height: 1;
    position: relative;
    z-index: 9;
}

body {
    font-family: 'Roboto', sans-serif;
}
body {
    line-height: 22px;
}
body {
    font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
    font-size: 14px;
    line-height: 1.42857143;
    color: #333;
    background-color: #fff;
}
html {
    font-size: 10px;
    -webkit-tap-highlight-color: rgba(0,0,0,0);
}
html {
    font-family: sans-serif;
    -webkit-text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
}
:before, :after {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}
:before, :after {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}
</style>
