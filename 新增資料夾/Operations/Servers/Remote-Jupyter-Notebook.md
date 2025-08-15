The below content is copied from <https://docs.anaconda.com/anaconda/user-guide/tasks/remote-jupyter-notebook/>

1. Launch Jupyter Notebook from remote server, selecting a port number for <PORT>:

   <div>

   <div>

   <div>

   <div>

   ```
   # Replace <PORT> with your selected port number
   jupyter notebook --no-browser --port=<PORT>
   
   ```

   </div>
   </div>
   </div>
   </div>For example, if you want to use port number 8080, you would run the following:

   <div>

   <div>

   <div>

   <div>

   ```
   jupyter notebook --no-browser --port=8080
   
   ```

   </div>
   </div>
   </div>
   </div>Or run the following command to launch with default port:

   <div>

   <div>

   <div>

   <div>

   ```
   jupyter notebook --no-browser
   
   ```

   </div>
   </div>
   </div>
   </div>**Please note the port setting**. You will need it in the next step.
2. You can access the notebook from your remote machine over SSH by setting up a SSH tunnel. Run the following command from your local machine:

   <div>

   <div>

   <div>

   <div>

   ```
   # Replace <PORT> with the port number you selected in the above step
   # Replace <REMOTE_USER> with the remote server username
   # Replace <REMOTE_HOST> with your remote server address
   ssh -L 8080:localhost:<PORT> <REMOTE_USER>@<REMOTE_HOST>
   
   ```

   </div>
   </div>
   </div>
   </div>The above command opens up a new SSH session in the terminal.
3. Open a browser from your local machine and navigate to <http://localhost:8080/>, the Jupyter Notebook web interface. Replace 8080 with your port number used in step 1.