<template>
<v-container>
    <SearchBar @search="handleSearch" style="width: 50%;" />
    <v-switch v-model="listView" label="Toggle List View" class="my-4" />

    <UserList :users="filtered_users" :listView="listView" @user-click="openUserDialog" />

    <!-- User Details Dialog -->
    <v-dialog v-model="dialog" max-width="500">
        <v-card>
            <v-card-title>
                <span class="text-h5">{{ selected_user.name }}</span>
            </v-card-title>

            <v-tabs v-model="tab" background-color="primary" dark>
                <v-tab value="Contact">Contact Info</v-tab>
                <v-tab value="Company">Company</v-tab>
                <v-tab value="Address">Address</v-tab>
            </v-tabs>

            <v-tabs-window v-model="tab">
                <v-tabs-window-item value="Contact">
                    <v-card-text>
                        <p><strong>Email:</strong> {{ selected_user.email }}</p>
                        <p><strong>Phone:</strong> {{ selected_user.phone }}</p>
                        <p><strong>Username:</strong> {{ selected_user.username }}</p>
                    </v-card-text>
                </v-tabs-window-item>
                <v-tabs-window-item value="Company">
                    <v-card-text>
                        <p><strong>Company Name:</strong> {{ selected_user.company?.name }}</p>
                        <p><strong>Catchphrase:</strong> {{ selected_user.company?.catchPhrase }}</p>
                        <p><strong>BS:</strong> {{ selected_user.company?.bs }}</p>
                    </v-card-text>
                </v-tabs-window-item>
                <v-tabs-window-item value="Address">
                    <v-card-text>
                        <p><strong>Street:</strong> {{ selected_user.address?.street }}</p>
                        <p><strong>Suite:</strong> {{ selected_user.address?.suite }}</p>
                        <p><strong>City:</strong> {{ selected_user.address?.city }}</p>
                        <p><strong>Zipcode:</strong> {{ selected_user.address?.zipcode }}</p>
                    </v-card-text>
                </v-tabs-window-item>
            </v-tabs-window>

            <v-card-actions>
                <v-spacer />
                <v-btn color="primary" text @click="dialog = false">Close</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

    <div v-if="loading" class="loading-overlay">
        <v-progress-circular indeterminate color="primary" size="50" />
    </div>
    <v-alert v-if="error" type="error" class="my-4">{{ error }}</v-alert>
</v-container>
</template>

<script>
import SearchBar from './SearchBar.vue';
import UserList from './UserList.vue';
import axios from 'axios';

export default {
    components: {
        SearchBar,
        UserList
    },
    data() {
        return {
            users: [],
            filtered_users: [],
            listView: false,
            loading: false,
            error: null,
            dialog: false,
            selected_user: {},
            tab: null,
        };
    },

    methods: {
        async fetchUsers() {
            this.loading = true;
            try {
                const {
                    data: users
                } = await axios.get('https://jsonplaceholder.typicode.com/users');
                this.users = users.map((user) => ({
                    ...user,
                    photo: `https://randomuser.me/api/portraits/women/${user.id}.jpg`,
                }));
                this.filtered_users = this.users;
            } catch (err) {
                this.error = 'Failed to fetch user data.';
            } finally {
                this.loading = false;
            }
        },

        handleSearch(query) {
            this.filtered_users = this.users.filter(
                (user) =>
                user.name.toLowerCase().includes(query.toLowerCase()) ||
                user.company.name.toLowerCase().includes(query.toLowerCase())
            );
        },

        openUserDialog(user) {
            this.selected_user = user;
            this.dialog = true;
        },
    },

    mounted() {
        this.fetchUsers();
    },
};
</script>

<style scoped>
.loading-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgba(255, 255, 255, 0.7);
}
</style>
