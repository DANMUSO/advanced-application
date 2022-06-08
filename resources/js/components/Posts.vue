<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <div class="card">
                    <div class="row card-header justify-content-center">
                   <div class="col-md-10">
                    <div >Posts Component</div>
                    </div>
                    <div class="col-md-2">
                    <button type="button" class="btn btn-success" @click="newModal">
                    Add Post 
                    </button>
                    
                    <!-- Modal -->
                    <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewTitle" aria-hidden="true">
                    <div class="modal-dialog modal-dialog-centered" role="document">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" v-show="!editmode" id="exampleModalLongTitle">Add post</h5>
                            <h5 class="modal-title" v-show="editmode" id="exampleModalLongTitle">Update post's details</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                         <form @submit.prevent="editmode ? updatePost() : addPost()">
                        <div class="form-row">
                            <div class="form-group col-md-6">
                            <label for="inputEmail4">Title</label>
                            <input type="text" name="title" v-model="form.title" class="form-control" placeholder="Title">
                            <div v-if="form.errors.has('title')" v-html="form.errors.get('title')" style="color:red;"/>
                            </div>
                            <div class="form-group col-md-6">
                            <label for="inputPassword4">Post Body</label>
                            <input type="text" name="body" v-model="form.body" class="form-control" placeholder="Post Body">
                           <div v-if="form.errors.has('body')" v-html="form.errors.get('body')" style="color:red;"/>
                            </div>
                        </div>
                        <button v-show="editmode" type="submit" class="btn btn-success">Update</button>
                        <button v-show="!editmode" type="submit" class="btn btn-primary">Submit</button>
                        </form>
                        </div>
                        </div>
                    </div>
                    </div>
                    </div>
                    </div>
                    <div class="card-body">
              <div id="example1_wrapper" 
              class="dataTables_wrapper dt-bootstrap4">
              <div class="row">
              <div class="col-sm-12 col-md-6">
             </div>
                  <div class="col-sm-12 col-md-6">
                  <div id="example1_filter" class="dataTables_filter">
                  <label>Search by ID:<input type="search" v-model="keyword" class="form-control form-control-sm" placeholder="Search by ID" aria-controls="example1"></label>
                  </div></div></div><div class="row"><div class="col-sm-12">
                  <div class="table-responsive">
                  <table id="example1" class="table table-bordered table-striped dataTable" role="grid" aria-describedby="example1_info">
                <thead>
                 <tr role="row">
                  <th>Id</th>
                  <th>Title</th>
                  <th>Body</th>
                  <th>User</th>
                  <th>Date</th>
                  <th>Action</th>
                  </tr>
                </thead>
                <tbody>
                <tr class="odd" v-for="post in posts" :key="post.id">
                    <td>{{post.id}}</td>
                    <td>{{post.title}}</td>
                    <td>{{post.body}}</td>
                    <td>{{post.user.name}}</td>
                    <td>{{post.created_at |myDate}}</td>
                    <td><button type="button" @click="EditModal(post)" class="btn btn-warning" aria-label="Left Align">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil-square" viewBox="0 0 16 16">
                    <path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/>
                    <path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"/>
                    </svg>
                    </button> 
                    <br>
                    <br>
                    <button @click="deletePost(post.id)" type="button" class="btn btn-danger" aria-label="Left Align">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
                    <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                    <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                    </svg></button></td>
                  </tr>
                </tbody>
                <tfoot>
                <tr role="row">
                  <th>Id</th>
                  <th>Title</th>
                  <th>Body</th>
                  <th>User</th>
                  <th>Date</th>
                  <th>Action</th>
                  </tr>
                </tfoot>
              </table>
              </div></div></div></div>
            </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                keyword: null,
                editmode: false,
                posts: {},
                form: new Form({
                    id : '',
                    title: '',
                    body: ''
                })
            }
        },
        watch: {
        keyword(after, before) {
            this.loadposts();
        }
        },
        methods: {
            updatePost() {
                this.$Progress.start();
                this.form.put('api/posts/'+this.form.id).then(() => {
                     toast.fire({
                    icon: 'success',
                    title: 'Updated successfully'
                })
                }).catch(() =>{
                    this.$Progress.fail();
                })
            },
            EditModal(post) {
            this.editmode = true;
            $('#addNew').modal('show');
            this.form.fill(post);
            },
            newModal() {
                this.editmode = false;
                $('#addNew').modal('show');
            },
            loadposts(){
                axios.get("api/posts", { params: { keyword: this.keyword } }).
                then(({ data }) => (this.posts = data.data))
            },
            addPost() {
                this.$Progress.start();
                this.form.post('api/posts').then(() =>{
                   toast.fire({
                    icon: 'success',
                    title: 'Added successfully'
                })
                }).catch(() => {
                    
                })
                this.$Progress.finish();
                
            },
            deletePost(id)
            {
                swal.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes, delete it!'
                }).then((result) => {
                   
                if (result.isConfirmed) {
                    this.form.delete('api/posts/'+id);
                    swal.fire(
                    'Deleted!',
                    'Post has been deleted.',
                    'success'
                    )
                }
                })

            }
        },
        created() {
            this.loadposts();
            setInterval(() => this.loadposts(), 4000);
        }
    }
</script>
