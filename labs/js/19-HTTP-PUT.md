# Lab 19: HTTP PUT

## Objectives

- [ ] Communicate with a REST API to update data

## Steps

### Communicate with a REST API to update data

1. **Implement** a **method** in the API object to do a **PUT** (update).

   #### `src\projects\ProjectAPI.js`

   ```diff
   + import { Project } from './Project';
   ...

   + function toProject(object) {
   +   return new Project(object);
   + }

   const projectAPI = {
     get(...){
       ...
     }
   + ,
   ...

   +  put(project) {
   +    return fetch(`${url}/${project.id}`, {
   +      method: 'PUT',
   +      body: JSON.stringify(project),
   +      headers: {
   +        'Content-Type': 'application/json'
   +      }
   +    })
   +      .then(checkStatus)
   +      .then(parseJSON)
   +      .then(toProject) // make sure there is no s on toProject
   +      .catch((error) => {
   +        console.log('log client error ' + error);
   +        throw new Error(
   +          'There was an error updating the project. Please try again.'
   +        );
   +      });
   +  }

   };
   ```

1. **Invoke** the **method** in your container (`ProjectsPage`) component.

   #### `src\projects\ProjectsPage.js`

   ```diff
   class ProjectsPage extends React.Component<any, ProjectsContainerState> {
     state = {
       projects: [],
       loading: false,
       error: undefined
     };

   ...

   saveProject = (project: Project) => {
   -    this.setState((previousState: ProjectsPageState) => {
   -      let projects = previousState.projects.map((p: Project) => {
   -        return p.id === project.id ? project : p;
   -      });
   -      return { projects };
   -    });

   +    projectAPI
   +      .put(project)
   +      .then(data => {
   +        this.setState(state => {
   +          let projects = state.projects.map(p => {
   +            return p.id === project.id ? project : p;
   +          });
   +          return { projects };
   +        });
   +      })
   +      .catch(error => {
   +        this.setState({ error: error.message });
   +      });
     };

   }

   export default ProjectsContainer;
   ```

1. Verify the application is working by following these steps.
   1. Click the edit button for a project.
   2. Change the project name in the form.
   3. Click the save button on the form.
   4. Verify the card shows the updated data.
   5. Refresh your browser.
   6. Verify the project is still updated.

![image](https://user-images.githubusercontent.com/1474579/65075658-573c3a80-d965-11e9-943c-32fa4f6b8849.png)

---

### &#10004; You have completed Lab 19
