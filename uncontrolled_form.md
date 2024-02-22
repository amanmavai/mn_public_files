```jsx
import React, { useState, useRef } from 'react';
import { Button, Drawer, List, ListItem, ListItemText, TextField, Chip, Autocomplete } from '@mui/material';

const ProjectItem = ({ project, onSelect, isSelected }) => (
  <ListItem button onClick={onSelect} selected={isSelected} className="mb-2">
    <ListItemText primary={project.name} secondary={project.description} />
  </ListItem>
);

const Editor = ({ projects, setProjects }) => {
  const [selectedProject, setSelectedProject] = useState(null);
  const [isDrawerOpen, setIsDrawerOpen] = useState(false);
  const [techStack, setTechStack] = useState([]);
  const [keyTeams, setKeyTeams] = useState([]);
  const formRef = useRef();

  const openDrawer = (project) => {
    setSelectedProject(project);
    setTechStack(project?.techStack || []);
    setKeyTeams(project?.keyTeams || []);
    setIsDrawerOpen(true);
  };

  const closeDrawer = () => {
    setIsDrawerOpen(false);
    setSelectedProject(null); // Clear the selected project
    setTechStack([]); // Reset tech stack
    setKeyTeams([]); // Reset key teams
  };

  const onSubmit = (event) => {
    event.preventDefault();
    const formData = new FormData(formRef.current);
    const data = Object.fromEntries(formData.entries());
    data.techStack = techStack;
    data.keyTeams = keyTeams;

    if (selectedProject) {
      // Edit existing project
      setProjects(projects.map((p) => (p === selectedProject ? { ...selectedProject, ...data } : p)));
    } else {
      // Add new project
      setProjects([...projects, data]);
    }
    closeDrawer();
  };

  const addNewProject = () => {
    setSelectedProject(null); // Clear selection for new project
    openDrawer({});
  };

  return (
    <div className="flex">
      <div className="w-1/2">
        <Button variant="contained" onClick={addNewProject} className="my-2">
          New Project
        </Button>
        <List>
          {projects.map((project, index) => (
            <ProjectItem
              key={index}
              project={project}
              onSelect={() => openDrawer(project)}
              isSelected={selectedProject === project}
            />
          ))}
        </List>
      </div>
      <Drawer anchor="right" open={isDrawerOpen} onClose={closeDrawer}>
        <form ref={formRef} onSubmit={onSubmit} className="p-4">
          <TextField name="name" label="Name" defaultValue={selectedProject?.name} fullWidth margin="normal" />
          <TextField name="description" label="Description" defaultValue={selectedProject?.description} fullWidth margin="normal" />
          <TextField name="aboutCustomer" label="About Customer" defaultValue={selectedProject?.aboutCustomer} fullWidth margin="normal" />
          <TextField name="keyReasons" label="Key Reasons" defaultValue={selectedProject?.keyReasons} fullWidth margin="normal" />
          <Autocomplete
            multiple
            id="tech-stack"
            options={[]}
            value={techStack}
            onChange={(event, newValue) => {
              setTechStack(newValue);
            }}
            freeSolo
            renderTags={(value, getTagProps) =>
              value.map((option, index) => (
                <Chip variant="outlined" label={option} {...getTagProps({ index })} />
              ))
            }
            renderInput={(params) => <TextField {...params} variant="filled" label="Tech Stack" placeholder="Add Tech" />}
          />
          <Autocomplete
            multiple
            id="key-teams"
            options={[]}
            value={keyTeams}
            onChange={(event, newValue) => {
              setKeyTeams(newValue);
            }}
            freeSolo
            renderTags={(value, getTagProps) =>
              value.map((option, index) => (
                <Chip variant="outlined" label={option} {...getTagProps({ index })} />
              ))
            }
            renderInput={(params) => <TextField {...params} variant="filled" label="Key Teams" placeholder="Add Team" />}
          />
          <Button type="submit" variant="contained" className="mt-4">
            Save
          </Button>
        </form>
      </Drawer>
    </div>
  );
};

export default Editor;

```
