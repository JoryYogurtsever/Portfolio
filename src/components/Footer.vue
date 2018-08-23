<template>
  <div class="bottomFooter">
    &copy Jory Hagen

    <a href="https://github.com/JoryYogurtsever">
      <img src="../../static/github-square-brands.svg" class="github" />
    </a>
    <a href="mailto:jrhagen@ualberta.ca"><i class="far fa-envelope mailer"></i></a>
  </div>
</template>

<style>
  .bottomFooter {
    background-color: gainsboro;
    height: 65px;
    /*width: 100%; */
    padding-left: 33%;
    padding-top: 30px;
    padding-bottom: 10px;
  }
  .github {
    padding-left: 60px;
    width: 25px;
  }
  .mailer {
    padding-left: 60px;
    font-size: 30px;
    color: black;
    padding-bottom: 10px;
  }
  @media (max-width:640px) {
    .bottomFooter {
      padding-left: 30%;
      font-size: 0.6em;
    }
    .github {
      padding-left: 20px;
      width: 15px;
    }
    .mailer {
      padding-left: 20px;
      font-size: 18px;
    }
  }
</style>
