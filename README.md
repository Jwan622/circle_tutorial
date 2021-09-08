The circle.yml is from this:

https://circleci.com/docs/2.0/project-walkthrough/

Some key notes:

The following describes the detail of the added key values:

The restore_cache: step searches for a cache with a key that matches the key template. The template begins with deps1- and embeds the current branch name using {{ .Branch }}. The checksum for the requirements.txt file is also embedded into the key template using {{ checksum "requirements/dev.txt" }}. CircleCI restores the most recent cache that matches the template, in this case the branch the cache was saved on and the checksum of the requirements/dev.txt file used to create the cached virtualenv must match.

The run: step named Install Python deps in a venv creates and activates a virtual environment in which to install the Python dependencies, as before.

The save_cache: step creates a cache from the specified paths, in this case venv. The cache key is created from the template specified by the key:. Note that it is important to use the same template as the restore_cache: step so that CircleCI saves a cache that can be found by the restore_cache: step. Before saving the cache CircleCI generates the cache key from the template, if a cache that matches the generated key already exists then CircleCI does not save a new cache. Since the template contains the branch name and the checksum of requirements/dev.txt, CircleCI will create a new cache whenever the job runs on a different branch, and/or if the checksum of requirements/dev.txt changes.

You can read more about caching [here](https://circleci.com/docs/2.0/caching/).


Each command runs in a new shell, so the virtual environment that was activated in the dependencies installation step is activated again in this final run: key with . venv/bin/activate.
The store_artifacts step is a special step. The path: is a directory relative to the project’s root directory where the files are stored. The destination: specifies a prefix chosen to be unique in the event that another step in the job produces artifacts in a directory with the same name. CircleCI collects and uploads the artifacts to S3 for storage.
When a job completes, artifacts appear in the CircleCI Artifacts tab:


