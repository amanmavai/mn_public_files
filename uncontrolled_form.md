```jsx
import React, { useRef } from 'react';
import { Select, MenuItem, InputLabel, FormControl, Button } from '@mui/material';

function App() {
  const technologiesRef = useRef(null);
  const teamsRef = useRef(null);

  const handleSubmit = (event) => {
    event.preventDefault();
    
    // For Material-UI Select, the value can be directly accessed from the ref
    const selectedTechnologies = technologiesRef.current.value;
    const selectedTeams = teamsRef.current.value;

    console.log("Selected Technologies:", selectedTechnologies);
    console.log("Selected Teams:", selectedTeams);
  };

  return (
    <form onSubmit={handleSubmit}>
      <FormControl fullWidth>
        <InputLabel id="technologies-label">Technologies</InputLabel>
        <Select
          labelId="technologies-label"
          multiple
          defaultValue={[]}
          inputRef={technologiesRef}
          label="Technologies"
        >
          <MenuItem value="react">React</MenuItem>
          <MenuItem value="vue">Vue</MenuItem>
          <MenuItem value="angular">Angular</MenuItem>
          {/* Add more technology options here */}
        </Select>
      </FormControl>
      <br /><br />
      <FormControl fullWidth>
        <InputLabel id="teams-label">Teams</InputLabel>
        <Select
          labelId="teams-label"
          multiple
          defaultValue={[]}
          inputRef={teamsRef}
          label="Teams"
        >
          <MenuItem value="frontend">Frontend</MenuItem>
          <MenuItem value="backend">Backend</MenuItem>
          <MenuItem value="devops">DevOps</MenuItem>
          {/* Add more team options here */}
        </Select>
      </FormControl>
      <br /><br />
      <Button type="submit" variant="contained">Submit</Button>
    </form>
  );
}

export default App;


```
