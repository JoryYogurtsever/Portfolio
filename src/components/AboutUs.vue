<template>
  <div id="aboutMeSection">
    <h1 class="lovelyTitle">The man behind the pixels</h1>
    <img src="../assets/JoryBOOM.png" class="lovelyImage">
    <p class="lovelyText">
      I used to joke that I'm Amish. I got my first cell
      phone when I was 20 years old in 2010 and that was
      only from necessity as I had started running my own
      construction company. I never envied people spending
      hours each day on social media and didn't think that
      computers had anything to offer me.
      That all changed as I was studying physics at university.
      I was probably the only person around who
      actually loved using Microsoft Excel, then we started using
      Matlab for data analysis. I'd spent my life in
      construction building anything my imagination would allow,
      and suddenly I saw how powerful of a tool
      computers could be. Within a year I had build my own computer
      from parts and was running Linux and toying with
      the command line. <br>
      </p>
    <img src="../assets/metTheOzge.jpg" class="lovelyImage">
    <p class="lovelyText">
      Three and a half years ago I met the Ozge she was probably
      eating yogurt at the time. We've spent the last
      three years travelling and teaching English. The lifestyle
      is great, but I am much too introverted to be a teacher.
      Six months ago I decided it was time for a change and
      remembered how much fun I'd had coding in Matlab back in
      university. This was when I had my Eureka moment,
      (I was walking to PizzaHut at the time). So I buckled down and
      got serious about learning HTML, CSS, Javascript and
      whatever else I seemed to need to know. I started making
      video games as a gift for my girlfriend and that is something
      I ran with as well, coding in C# a bit as well. <br>
    </p>
    <img src="../assets/AboveTheClouds.png" class="lovelyImage">
    <p class="lovelyText">
      Learning to code has been a bit like climbing a mountain
      that gets taller right when you almost get to the top.
      Just when you have the hang of Javascript, then you realize
      you need to know how to use Webpack or Vue or React
      or whatever else it is that you weren't expecting. It feels
      like I finally broke through the clouds, I have three sites
      live now and have got several contracts for my custom games.
      Now I am ready to look for a job.
    </p>
    <hr class="lovelyLine">
    <div class="lovelyTitle"><h1 id="contactSection">Contact Us</h1></div>
  </div>
</template>

<style>
  .lovelyTitle {
    color: olive;
    margin: 20px;
  }
  .lovelyImage {
    width: 50%;
    margin-left: 22%;
    padding: 10px;
    border: 2px solid seagreen;
    border-radius: 5px;
  }
  .lovelyText {
    margin: 20px;
    color: forestgreen;
  }
  .lovelyLine {
    border-color: seagreen;
    height: 2px;
    background-color: seagreen;
  }
  @media (max-width:640px) {
    .lovelyText {
      font-size: 10px;
    }
    .lovelyTitle {
      font-size: 1.2em;
      margin: 5px;
    }
    #contactSection {
      font-size: 1.2em;
      margin: 5px;
    }
  }
</style>
