<script setup lang="ts">
  import {
    actionSheetController,
    IonContent,
    IonCol,
    IonFab,
    IonFabButton,
    IonGrid,
    IonPage,
    IonHeader,
    IonIcon,
    IonImg,
    IonRow,
    IonTitle,
    IonToolbar,
  } from "@ionic/vue";
  import { camera, trash, close } from "ionicons/icons";
  import { usePhotoGallery, UserPhoto } from "@/composables/usePhotoGallery";

  const { takePhoto, deletePhoto, photos } = usePhotoGallery();

  const showActionSheet = async (photo: UserPhoto) => {
    const actionSheet = await actionSheetController.create({
      header: "Photos",
      buttons: [
        {
          text: "Delete",
          role: "destructive",
          icon: trash,
          handler: () => {
            deletePhoto(photo);
          },
        },
        {
          text: "Cancel",
          icon: close,
          role: "cancel",
          handler: () => {
            // Nothing to do, action sheet is automatically closed
          },
        },
      ],
    });
    await actionSheet.present();
  };
</script>

<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Photo Gallery</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Photo Gallery 2</ion-title>
        </ion-toolbar>
      </ion-header>
    </ion-content>

    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col
            size="6"
            :key="photo.filepath"
            v-for="photo in photos">
            <ion-img
              :src="photo.webviewPath"
              @click="showActionSheet(photo)"></ion-img>
          </ion-col>
        </ion-row>
      </ion-grid>

      <ion-fab
        vertical="bottom"
        horizontal="center"
        slot="fixed">
        <ion-fab-button @click="takePhoto()">
          <ion-icon :icon="camera"></ion-icon>
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>
