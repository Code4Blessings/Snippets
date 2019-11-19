# THE SEARCH BAR
--Taken from Lambda School Sprint Challenge for Single Page Applications (JSX) suing the Rick and Morty API

#Search Form
import React from "react";
import styled from "styled-components";
​
const LabelStyle = styled.label`
  font-size: 30px;
  color: #000;
`;
​
const Input = styled.input`
  width: 220px;
  padding: 15px 22px;
  margin: 10px 5px;
  box-sizing: border-box;  
  border: 1px solid #000;
  border-radius: 4px;
  font-size: 15px;
`;
​
 function SearchForm(props) {
  return (
    <section className = "search-form">
      <form>
        <LabelStyle htmlFor = "name">Search:</LabelStyle>
        <Input 
          type='search' 
          placeholder = 'search rick and morty'
          onChange = {(e) => props.search(e.target.value)} />
      </form>
    </section>
  );
} 
  
export default SearchForm;

#CharacterList (Third Party API call with axios) with Search Form imported

import React, { useEffect, useState } from "react";
import axios from "axios";
import SearchForm from './SearchForm';
import CharacterCard from "./CharacterCard";

function CharacterList() {
  // TODO: Add useState to track data from useEffect
  const [chars, setChars] = useState([]);
  const [filterChars, setFilterChars] = useState([]);
  const search = (val) => {
    if (!val) {
      setFilterChars(chars);
    } else {
      const filteredArr = chars.filter(char => {
        return char["name"].toLowerCase().search(val.toLowerCase()) > -1;
      })
      setFilterChars(filteredArr);
    }
  }
  useEffect(() => {
    //const id = props.match.params.id
    // TODO: Add API Request here - must run in `useEffect`
    //  Important: verify the 2nd `useEffect` parameter: the dependancies array!
    axios
      .get('https://rickandmortyapi.com/api/character/')
      .then((response) => {
        setChars(response.data.results);
        setFilterChars(response.data.results);
      })
      .catch((error) => {
        console.log('Data returned an error', error)
      })
  },[]);
  return (
    <section className="character-list">
      <SearchForm search = {search} />
      {filterChars.map(char => (
        <CharacterCard
          key = {char.id}
          char = {char} />
      ))}
    </section>
  );
};
export default CharacterList;

#For the entire source code for this application, see https://github.com/Code4Blessings/Sprint-Challenge-Single-Page-Apps