<template>
  <div class="gretting">
    <div class="gretting__text">
      <h1>Hi, Skylar Dias</h1>
      <h3>Let's finish your task today!</h3>
    </div>
    <div class="gretting__user">
      <img src="../assets/mock_user.png" alt="user profil image" />
    </div>
  </div>
</template>

<style lang="scss">
.gretting {
  display: flex;
  flex-direction: row;
  justify-content: space-between;

  &__text {
    h1 {
      color: get-color(secondary, 500);
    }

    h3 {
      margin-top: 20px;
      color: get-color(secondary, 400);
    }
  }

  &__user {
    display: flex;
    flex-direction: row;
    align-items: center;

    img {
      width: 52px;
      height: 52px;
      border-radius: 100px;
    }
  }
}
</style>
