<template>
    <div class="container">
        <div class="row mt-5">
          <div class="col-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Responsive Hover Table</h3>

                <div class="card-tools">
                  <button class="btn btn-success" data-toggle="modal" data-target="#exampleModal">Add New User</button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover text-nowrap">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Email</th>
                      <th>Type</th>
                      <th>Created At</th>
                      <th>Tools</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="user in users" :key="user.id">
                      <td>{{user.id}}</td>
                      <td>{{user.name}}</td>
                      <td>{{user.email}}</td>
                      <td><span class="tag tag-success">{{user.type | upText}}</span></td>
                      <td>{{user.created_at | myDate}}</td>
                      <td><a href="" class="">
                          <i class="fas fa-edit"></i></a>
                          <a href="" class="">
                              <i class="fas fa-trash"></i>
                          </a>
                          </td>
                    </tr>
                    
                  </tbody>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
        </div>
        <!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Create New User</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <form @submit.prevent="createUser">
      <div class="modal-body">
        <div class="form-group">
            <input v-model="form.name" type="text" name="name"
            placeholder="Name"
            class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
          <has-error :form="form" field="name"></has-error>
        </div>
        <div class="form-group">
            <input v-model="form.email" type="email" name="email"
            placeholder="Email"
            class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
          <has-error :form="form" field="email"></has-error>
        </div>

        <div class="form-group">
            <textarea v-model="form.bio"  name="bio"
            placeholder="Bio (Optional)"
            class="form-control" :class="{ 'is-invalid': form.errors.has('email') }"></textarea>
          <has-error :form="form" field="bio"></has-error>
        </div>

        <div class="form-group">
            <select v-model="form.type"  name="type"
            class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
              <option value="">Select User Role</option>
              <option value="admin">Admin</option>
              <option value="user">General User</option>
              <option value="author">Author</option>
            </select>
          <has-error :form="form" field="bio"></has-error>
        </div>
        <div class="form-group">
            <input v-model="form.password" type="password" name="password"
            placeholder="Password"
            class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
          <has-error :form="form" field="password"></has-error>
        </div>

        
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
        <button type="submit" class="btn btn-primary">Create User</button>
      </div>
      </form>
    </div>
    
  </div>
</div>
    </div>
</template>

<script>
    export default {
      data () {
        return {
          users: {},
      // Create a new form instance
          form: new Form({
            name: '',
            email: '',
            password: '',
            type: '',
            bio: '',
            photo: ''
      })
    }
  },
  methods:{
      loadUsers(){
        axios.get('api/user').then( ({ data }) => (this.users = data.data) )
      },
      createUser(){
        this.$Progress.start();
        this.form.post('api/user');

        $('#exampleModal').modal('hide');
        Toast.fire({
          icon: 'success',
          title: 'User Created Successfully.'
        });

        this.$Progress.finish();
      }
  },
        created() {
            this.loadUsers();
            console.log('Component mounted.')
        }
    }
</script>
