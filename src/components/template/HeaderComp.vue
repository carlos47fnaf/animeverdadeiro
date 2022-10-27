<style></style>
<template>
  <header>
    <div class="esquerdo">Animes<strong>Vue</strong></div>
   
    <div class="direita">Perfil</div>
  </header>
</template>
<style scoped>
header {
  justify-content: space-between;
  display: flex;
  align-items: center;
  height: 90px;
  background: #18171d;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}
a {
  text-decoration: none;
  color: white;
}
.esquerdo{
  margin-left: 20px;
  font-size: 30px;
}

strong{
  color: #ff0b53;
}
.direita{
  margin-right: 20px;
}

input {
  width: 300px;
  height: 30px;
  display: flex;
  align-content: center;
  
}

</style>
