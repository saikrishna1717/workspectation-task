<template>
<v-row v-if="!listView">
    <v-col cols="12" md="6" lg="4" v-for="user in users" :key="user.id">
        <v-card @click="$emit('user-click', user)">
            <v-card-title>
                <v-avatar class="me-3">
                    <v-img :src="user.photo" alt="User Photo" />
                </v-avatar>
                <span>{{ user.name }}</span>
            </v-card-title>

            <v-card-subtitle>
                <span class="text-subtitle-2">Username: {{ user.username }}</span>
            </v-card-subtitle>

            <v-card-text>
                <p>Email: {{ user.email }}</p>
                <p>Phone: {{ user.phone }}</p>
                <p>Company: {{ user.company.name }}</p>
            </v-card-text>

            <v-card-actions>
                <v-chip color="primary" outlined>
                    {{ user.address.city }}
                </v-chip>
            </v-card-actions>
        </v-card>
    </v-col>
</v-row>

<v-list v-else>
    <v-list-item v-for="user in users" :key="user.id" @click="$emit('user-click', user)">
        <v-avatar>
            <v-img :src="user.photo" />
        </v-avatar>
        <v-list-item-content>
            <v-list-item-title>{{ user.name }}</v-list-item-title>
            <v-list-item-subtitle>{{ user.company.name }}</v-list-item-subtitle>
        </v-list-item-content>
    </v-list-item>
</v-list>
</template>

  
<script>
export default {
    props: ['users', 'listView'],
};
</script>

  
<style scoped>
  </style>
