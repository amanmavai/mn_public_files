```jsx
import React, { useState, useRef } from 'react';
import { Button, Drawer, List, ListItem, ListItemText, TextField, Chip } from '@mui/material';
import { useForm } from 'react-hook-form';

// ProjectItem component for displaying each project
const ProjectItem = ({ project, onSelect, isSelected }) => (
  <ListItem button onClick={onSelect} selected={isSelected} className="mb-2">
    <ListItemText primary={project.name} secondary={project.description} />
  </ListItem>
);

const Editor = ({ projects, setProjects }) => {
  const [selectedProject, setSelectedProject] = useState(null);
  const [isDrawerOpen, setIsDrawerOpen] = useState(false);
  const { register, handleSubmit, reset } = useForm();

  const openDrawer = (project) => {
    setSelectedProject(project);
    setIsDrawerOpen(true);
  };

  const closeDrawer = () => {
    setIsDrawerOpen(false);
    reset(); // Reset form fields
  };

  const onSubmit = (data) => {
    if (selectedProject) {
      // Edit existing project
      setProjects(
        projects.map((p) => (p === selectedProject ? { ...selectedProject, ...data } : p))
      );
    } else {
      // Add new project
      setProjects([...projects, data]);
    }
    closeDrawer();
  };

  const addNewProject = () => {
    setSelectedProject(null); // Clear selection for new project
    openDrawer();
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
        <form onSubmit={handleSubmit(onSubmit)} className="p-4">
          <TextField {...register("name")} label="Name" defaultValue={selectedProject?.name} fullWidth margin="normal" />
          <TextField {...register("description")} label="Description" defaultValue={selectedProject?.description} fullWidth margin="normal" />
          <TextField {...register("aboutCustomer")} label="About Customer" defaultValue={selectedProject?.aboutCustomer} fullWidth margin="normal" />
          <TextField {...register("keyReasons")} label="Key Reasons" defaultValue={selectedProject?.keyReasons} fullWidth margin="normal" />
          {/* Implement TechStack and KeyTeams inputs */}
          <Button type="submit" variant="contained" className="mt-4">
            Save
          </Button>
        </form>
      </Drawer>
    </div>
  );
};

export default Editor;



const Editor = ({ projects, setProjects }) => {
  const [selectedProject, setSelectedProject] = useState(null);
  const [isDrawerOpen, setIsDrawerOpen] = useState(false);
  const [techStack, setTechStack] = useState([]);
  const [keyTeams, setKeyTeams] = useState([]);

  // Resetting form state when the drawer is opened
  const openDrawer = (project) => {
    setSelectedProject(project);
    setTechStack(project?.techStack || []);
    setKeyTeams(project?.keyTeams || []);
    setIsDrawerOpen(true);
  };

  const closeDrawer = () => {
    setIsDrawerOpen(false);
    setSelectedProject(null); // Clear the selected project
    setTechStack([]); // Clear tech stack
    setKeyTeams([]); // Clear key teams
  };

  const onSubmit = (data) => {
    const projectData = { ...data, techStack, keyTeams };

    if (selectedProject) {
      // Edit existing project
      setProjects(projects.map((p) => (p === selectedProject ? { ...selectedProject, ...projectData } : p)));
    } else {
      // Add new project
      setProjects([...projects, projectData]);
    }
    closeDrawer();
  };

  // Additional UI and form logic remains unchanged

  return (
    <div className="flex">
      {/* Existing UI elements */}
      <Drawer anchor="right" open={isDrawerOpen} onClose={closeDrawer}>
        <form onSubmit={handleSubmit(onSubmit)} className="p-4">
          {/* Existing form fields */}
          <Autocomplete
            multiple
            id="tech-stack"
            options={[]} // You should populate this with possible tech stack options
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
            options={[]} // Populate with possible team options
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



```
