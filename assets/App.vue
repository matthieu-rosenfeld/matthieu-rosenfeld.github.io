<template>
  <div id="wrapper">
    <header>
      <h1 >The feed</h1>
      <nav>
        <div >Les membres</div>
        <div >S'inscrire</div>
        <div >Se connecter</div>
      </nav>
    </header> 
    <main>
      <router-view />
    </main>
  </div>
</template>


<style scoped>
  #wrapper{
    display:flex;
    flex-direction: column;
    align-items:center;
    min-height: 100vh;
    background-color: rgb(225, 235, 250);
  }
  header{
    width: 100%;
    position:sticky;
    top:0px;
    background-color: rgb(100, 210, 250);
    padding: 20px;
  }
  header h1{
    text-align: center;
    font-family: helvetica, serif;
    font-weight: 700;
  }
  nav{
    box-shadow: 0 0 0.5rem #999;
    display:flex;
    justify-content:space-evenly;
    width:80%;
    margin: 0 auto;
  }

  nav >div{
    padding:10px;  
    background-color: rgb(105, 190, 250);
    flex-grow:1;
    text-align: center;
    border: solid #22A 1px;
  }
  nav >div:hover{
    box-shadow: 0 0 0.3rem #000;
    cursor: pointer;
  }
  main{
    max-width: 1280px;
    width:780px;
    padding: 10px;
    background-color: rgb(225, 240, 255);
    flex-grow:1;
  }

  @media (max-width: 800px) {
    main {
      width:80%;
    }
  }
 </style>

