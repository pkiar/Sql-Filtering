{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Filtering Data with SQL - Lab"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Introduction \n",
    "\n",
    "NASA wants to go to Mars! Before they build their rocket, NASA needs to track information about all of the planets in the Solar System. In this lab, you'll practice querying the database with various `SELECT` statements. This will include selecting different columns and implementing other SQL clauses like `WHERE` to return the data desired."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<img src=\"https://raw.githubusercontent.com/learn-co-curriculum/dsc-filtering-lab-v2-4/master/images/planets.png\" alt=\"image of solar system\" width=\"600\">"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Objectives\n",
    "\n",
    "You will practice the following:\n",
    "\n",
    "* Retrieve a subset of records from a table using a `WHERE` clause\n",
    "* Filter results using conditional operators such as `BETWEEN`, `IS NULL`, and `LIKE`\n",
    "* Apply an aggregate function to the result of a filtered query"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Connecting to the Database\n",
    "\n",
    "To get started, import `sqlite3` as well as `pandas` for conveniently displaying results. Then, connect to the SQLite database located at `planets.db`. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [],
   "source": [
    "import sqlite3\n",
    "import pandas as pd\n",
    "\n",
    "con = sqlite3.connect(\"planets.db\")\n",
    "cursor = con.cursor()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "type",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "name",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "tbl_name",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "rootpage",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "sql",
         "rawType": "object",
         "type": "string"
        }
       ],
       "ref": "0c497d7f-8b40-4c6e-abbd-f43000888bff",
       "rows": [
        [
         "0",
         "table",
         "planets",
         "planets",
         "2",
         "CREATE TABLE planets\n                                            (id INTEGER PRIMARY KEY,\n                                                 name TEXT,\n                                                 color TEXT,\n                                                 num_of_moons INTEGER,\n                                                 mass REAL, rings BOOLEAN)"
        ]
       ],
       "shape": {
        "columns": 5,
        "rows": 1
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>type</th>\n",
       "      <th>name</th>\n",
       "      <th>tbl_name</th>\n",
       "      <th>rootpage</th>\n",
       "      <th>sql</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>table</td>\n",
       "      <td>planets</td>\n",
       "      <td>planets</td>\n",
       "      <td>2</td>\n",
       "      <td>CREATE TABLE planets\\n                        ...</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    type     name tbl_name  rootpage  \\\n",
       "0  table  planets  planets         2   \n",
       "\n",
       "                                                 sql  \n",
       "0  CREATE TABLE planets\\n                        ...  "
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "all_table = \"\"\"\n",
    "             SELECT *\n",
    "             FROM sqlite_master\n",
    "            \"\"\"\n",
    "pd.read_sql_query(all_table,con)            "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "name",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "type",
         "rawType": "object",
         "type": "string"
        }
       ],
       "ref": "35545bbc-ac2d-4e51-ba78-d94f17dcab47",
       "rows": [
        [
         "0",
         "id",
         "INTEGER"
        ],
        [
         "1",
         "name",
         "TEXT"
        ],
        [
         "2",
         "color",
         "TEXT"
        ],
        [
         "3",
         "num_of_moons",
         "INTEGER"
        ],
        [
         "4",
         "mass",
         "REAL"
        ],
        [
         "5",
         "rings",
         "BOOLEAN"
        ]
       ],
       "shape": {
        "columns": 2,
        "rows": 6
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>name</th>\n",
       "      <th>type</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>id</td>\n",
       "      <td>INTEGER</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>name</td>\n",
       "      <td>TEXT</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>color</td>\n",
       "      <td>TEXT</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>num_of_moons</td>\n",
       "      <td>INTEGER</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>mass</td>\n",
       "      <td>REAL</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>rings</td>\n",
       "      <td>BOOLEAN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           name     type\n",
       "0            id  INTEGER\n",
       "1          name     TEXT\n",
       "2         color     TEXT\n",
       "3  num_of_moons  INTEGER\n",
       "4          mass     REAL\n",
       "5         rings  BOOLEAN"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "query1 = \"\"\"\n",
    "            PRAGMA table_info(\"planets\");  \n",
    "         \"\"\"\n",
    "column_names = pd.read_sql_query(query1,con)     \n",
    "column_names[[\"name\",\"type\"]]       "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Database Schema\n",
    "\n",
    "This database contains a single table, `planets`. This is the schema:\n",
    "\n",
    "```\n",
    "CREATE TABLE planets (\n",
    "  id INTEGER PRIMARY KEY,\n",
    "  name TEXT,\n",
    "  color TEXT,\n",
    "  num_of_moons INTEGER,\n",
    "  mass REAL,\n",
    "  rings BOOLEAN\n",
    ");\n",
    "```\n",
    "\n",
    "The data looks something like this:\n",
    "\n",
    "| id | name    | color      | num_of_moons | mass   | rings |\n",
    "| -- | ------- | ---------- | ------------ | ------ | ----- |\n",
    "| 1  | Mercury | gray       | 0            | 0.55   | FALSE |\n",
    "| 2  | Venus   | yellow     | 0            | 0.82   | FALSE |\n",
    "| 3  | Earth   | blue       | 1            | 1.00   | FALSE |\n",
    "| 4  | Mars    | red        | 2            | 0.11   | FALSE |\n",
    "| 5  | Jupiter | orange     | 67           | 317.90 | FALSE |\n",
    "| 6  | Saturn  | hazel      | 62           | 95.19  | TRUE  |\n",
    "| 7  | Uranus  | light blue | 27           | 14.54  | TRUE  |\n",
    "| 8  | Neptune | dark blue  | 14           | 17.15  | TRUE  |"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## SQL Queries\n",
    "\n",
    "Write SQL queries for each of the statements below using the same pandas wrapping syntax from the previous lesson."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 1. Select just the `name` and `color` of each planet"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "Planet Name",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "Planet Colour",
         "rawType": "object",
         "type": "string"
        }
       ],
       "ref": "b7f2d403-3d3f-45b3-ab8a-795405630a41",
       "rows": [
        [
         "0",
         "Mercury",
         "gray"
        ],
        [
         "1",
         "Venus",
         "yellow"
        ],
        [
         "2",
         "Earth",
         "blue"
        ],
        [
         "3",
         "Mars",
         "red"
        ],
        [
         "4",
         "Jupiter",
         "orange"
        ],
        [
         "5",
         "Saturn",
         "hazel"
        ],
        [
         "6",
         "Uranus",
         "light blue"
        ],
        [
         "7",
         "Neptune",
         "dark blue"
        ]
       ],
       "shape": {
        "columns": 2,
        "rows": 8
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Planet Name</th>\n",
       "      <th>Planet Colour</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Mercury</td>\n",
       "      <td>gray</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Venus</td>\n",
       "      <td>yellow</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Earth</td>\n",
       "      <td>blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Mars</td>\n",
       "      <td>red</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Jupiter</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Saturn</td>\n",
       "      <td>hazel</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Uranus</td>\n",
       "      <td>light blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>Neptune</td>\n",
       "      <td>dark blue</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  Planet Name Planet Colour\n",
       "0     Mercury          gray\n",
       "1       Venus        yellow\n",
       "2       Earth          blue\n",
       "3        Mars           red\n",
       "4     Jupiter        orange\n",
       "5      Saturn         hazel\n",
       "6      Uranus    light blue\n",
       "7     Neptune     dark blue"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "name_and_colour = \"\"\"\n",
    "                   SELECT name AS \"Planet Name\" , color AS \"Planet Colour\"\n",
    "                   FROM planets\n",
    "                  \"\"\"\n",
    "pd.read_sql_query(name_and_colour,con)                  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 2. Select all columns for each planet whose `num_of_moons` is 0"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "id",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "name",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "color",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "num_of_moons",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "mass",
         "rawType": "float64",
         "type": "float"
        },
        {
         "name": "rings",
         "rawType": "int64",
         "type": "integer"
        }
       ],
       "ref": "f0d6f22c-7b58-4631-91cf-40a329ae9a5d",
       "rows": [
        [
         "0",
         "1",
         "Mercury",
         "gray",
         "0",
         "0.55",
         "0"
        ],
        [
         "1",
         "2",
         "Venus",
         "yellow",
         "0",
         "0.82",
         "0"
        ]
       ],
       "shape": {
        "columns": 6,
        "rows": 2
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "      <th>num_of_moons</th>\n",
       "      <th>mass</th>\n",
       "      <th>rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>Mercury</td>\n",
       "      <td>gray</td>\n",
       "      <td>0</td>\n",
       "      <td>0.55</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>Venus</td>\n",
       "      <td>yellow</td>\n",
       "      <td>0</td>\n",
       "      <td>0.82</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   id     name   color  num_of_moons  mass  rings\n",
       "0   1  Mercury    gray             0  0.55      0\n",
       "1   2    Venus  yellow             0  0.82      0"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "number_of_moons = \"\"\"\n",
    "                    SELECT *\n",
    "                    FROM planets\n",
    "                    WHERE num_of_moons = 0\n",
    "                  \"\"\"\n",
    "pd.read_sql_query(number_of_moons,con)                  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 3. Select the `name` and `mass` of each planet whose `name` has exactly 7 letters"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "Name of Plane",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "Mass of Planet",
         "rawType": "float64",
         "type": "float"
        }
       ],
       "ref": "4ae8320e-6cb6-44c0-8ee2-e81c594b2bac",
       "rows": [
        [
         "0",
         "Mercury",
         "0.55"
        ],
        [
         "1",
         "Jupiter",
         "317.9"
        ],
        [
         "2",
         "Neptune",
         "17.15"
        ]
       ],
       "shape": {
        "columns": 2,
        "rows": 3
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Name of Plane</th>\n",
       "      <th>Mass of Planet</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Mercury</td>\n",
       "      <td>0.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Jupiter</td>\n",
       "      <td>317.90</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Neptune</td>\n",
       "      <td>17.15</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  Name of Plane  Mass of Planet\n",
       "0       Mercury            0.55\n",
       "1       Jupiter          317.90\n",
       "2       Neptune           17.15"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "number_of_letters = \"\"\"\n",
    "                    SELECT name AS \"Name of Plane\", Mass AS \"Mass of Planet\"\n",
    "                    FROM planets\n",
    "                    WHERE LENGTH(name) = 7\n",
    "                  \"\"\"\n",
    "pd.read_sql_query(number_of_letters,con)  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 4. Select all columns for each planet whose `mass` is greater than 1.00"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "id",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "name",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "color",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "num_of_moons",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "mass",
         "rawType": "float64",
         "type": "float"
        },
        {
         "name": "rings",
         "rawType": "int64",
         "type": "integer"
        }
       ],
       "ref": "909ace4d-5d6b-40bc-a6d2-614916d1f36f",
       "rows": [
        [
         "0",
         "5",
         "Jupiter",
         "orange",
         "68",
         "317.9",
         "0"
        ],
        [
         "1",
         "6",
         "Saturn",
         "hazel",
         "62",
         "95.19",
         "1"
        ],
        [
         "2",
         "7",
         "Uranus",
         "light blue",
         "27",
         "14.54",
         "1"
        ],
        [
         "3",
         "8",
         "Neptune",
         "dark blue",
         "14",
         "17.15",
         "1"
        ]
       ],
       "shape": {
        "columns": 6,
        "rows": 4
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "      <th>num_of_moons</th>\n",
       "      <th>mass</th>\n",
       "      <th>rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>5</td>\n",
       "      <td>Jupiter</td>\n",
       "      <td>orange</td>\n",
       "      <td>68</td>\n",
       "      <td>317.90</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>6</td>\n",
       "      <td>Saturn</td>\n",
       "      <td>hazel</td>\n",
       "      <td>62</td>\n",
       "      <td>95.19</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>7</td>\n",
       "      <td>Uranus</td>\n",
       "      <td>light blue</td>\n",
       "      <td>27</td>\n",
       "      <td>14.54</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>8</td>\n",
       "      <td>Neptune</td>\n",
       "      <td>dark blue</td>\n",
       "      <td>14</td>\n",
       "      <td>17.15</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   id     name       color  num_of_moons    mass  rings\n",
       "0   5  Jupiter      orange            68  317.90      0\n",
       "1   6   Saturn       hazel            62   95.19      1\n",
       "2   7   Uranus  light blue            27   14.54      1\n",
       "3   8  Neptune   dark blue            14   17.15      1"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "the_mass= \"\"\"\n",
    "                    SELECT *\n",
    "                    FROM planets\n",
    "                    WHERE mass > 1\n",
    "                  \"\"\"\n",
    "pd.read_sql_query(the_mass,con)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 5. Select the `name` and `mass` of each planet whose `mass` is less than or equal to 1.00"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "Name of Plane",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "Mass of Planet",
         "rawType": "float64",
         "type": "float"
        }
       ],
       "ref": "2358c936-771a-4506-b333-40db689a1487",
       "rows": [
        [
         "0",
         "Mercury",
         "0.55"
        ],
        [
         "1",
         "Venus",
         "0.82"
        ],
        [
         "2",
         "Earth",
         "1.0"
        ],
        [
         "3",
         "Mars",
         "0.11"
        ]
       ],
       "shape": {
        "columns": 2,
        "rows": 4
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Name of Plane</th>\n",
       "      <th>Mass of Planet</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Mercury</td>\n",
       "      <td>0.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Venus</td>\n",
       "      <td>0.82</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Earth</td>\n",
       "      <td>1.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Mars</td>\n",
       "      <td>0.11</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  Name of Plane  Mass of Planet\n",
       "0       Mercury            0.55\n",
       "1         Venus            0.82\n",
       "2         Earth            1.00\n",
       "3          Mars            0.11"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "the_mass1= \"\"\"\n",
    "            SELECT name AS \"Name of Plane\", Mass AS \"Mass of Planet\"\n",
    "            FROM planets\n",
    "            WHERE mass <= 1\n",
    "            \"\"\"\n",
    "pd.read_sql_query(the_mass1,con)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 6. Select the `name` and `mass` of each planet whose `mass` is between 0 and 50"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "Name of Plane",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "Mass of Planet",
         "rawType": "float64",
         "type": "float"
        }
       ],
       "ref": "f9fdbadd-1a9a-448b-ae1c-a82a1aa3fffd",
       "rows": [
        [
         "0",
         "Mercury",
         "0.55"
        ],
        [
         "1",
         "Venus",
         "0.82"
        ],
        [
         "2",
         "Earth",
         "1.0"
        ],
        [
         "3",
         "Mars",
         "0.11"
        ],
        [
         "4",
         "Uranus",
         "14.54"
        ],
        [
         "5",
         "Neptune",
         "17.15"
        ]
       ],
       "shape": {
        "columns": 2,
        "rows": 6
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Name of Plane</th>\n",
       "      <th>Mass of Planet</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Mercury</td>\n",
       "      <td>0.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Venus</td>\n",
       "      <td>0.82</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Earth</td>\n",
       "      <td>1.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Mars</td>\n",
       "      <td>0.11</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Uranus</td>\n",
       "      <td>14.54</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Neptune</td>\n",
       "      <td>17.15</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  Name of Plane  Mass of Planet\n",
       "0       Mercury            0.55\n",
       "1         Venus            0.82\n",
       "2         Earth            1.00\n",
       "3          Mars            0.11\n",
       "4        Uranus           14.54\n",
       "5       Neptune           17.15"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "the_mass2= \"\"\"\n",
    "            SELECT name AS \"Name of Plane\", Mass AS \"Mass of Planet\"\n",
    "            FROM planets\n",
    "            WHERE mass BETWEEN 0 AND 50\n",
    "            \"\"\"\n",
    "pd.read_sql_query(the_mass2,con)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 7. Select all columns for planets that have at least one moon and a `mass` less than 1.00\n",
    "\n",
    "***Hint:*** You can use `AND` to chain together two conditions in SQL, similar to `and` in Python"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "id",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "name",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "color",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "num_of_moons",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "mass",
         "rawType": "float64",
         "type": "float"
        },
        {
         "name": "rings",
         "rawType": "int64",
         "type": "integer"
        }
       ],
       "ref": "462fe1e1-bca7-44a8-8fb6-384810a58453",
       "rows": [
        [
         "0",
         "4",
         "Mars",
         "red",
         "2",
         "0.11",
         "0"
        ]
       ],
       "shape": {
        "columns": 6,
        "rows": 1
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "      <th>num_of_moons</th>\n",
       "      <th>mass</th>\n",
       "      <th>rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>4</td>\n",
       "      <td>Mars</td>\n",
       "      <td>red</td>\n",
       "      <td>2</td>\n",
       "      <td>0.11</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   id  name color  num_of_moons  mass  rings\n",
       "0   4  Mars   red             2  0.11      0"
      ]
     },
     "execution_count": 34,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "the_query= \"\"\"\n",
    "            SELECT *\n",
    "            FROM planets\n",
    "            WHERE num_of_moons >=1 AND mass <1\n",
    "            \"\"\"\n",
    "pd.read_sql_query(the_query,con)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 8. Select the `name` and `color` of planets that have a `color` containing the string \"blue\""
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "Name of Plane",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "Color of Planet",
         "rawType": "object",
         "type": "string"
        }
       ],
       "ref": "c77adfb9-d05e-48fa-be08-9e1d3da481a5",
       "rows": [
        [
         "0",
         "Earth",
         "blue"
        ],
        [
         "1",
         "Uranus",
         "light blue"
        ],
        [
         "2",
         "Neptune",
         "dark blue"
        ]
       ],
       "shape": {
        "columns": 2,
        "rows": 3
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Name of Plane</th>\n",
       "      <th>Color of Planet</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Earth</td>\n",
       "      <td>blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Uranus</td>\n",
       "      <td>light blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Neptune</td>\n",
       "      <td>dark blue</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  Name of Plane Color of Planet\n",
       "0         Earth            blue\n",
       "1        Uranus      light blue\n",
       "2       Neptune       dark blue"
      ]
     },
     "execution_count": 50,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "the_color_blue = \"\"\"\n",
    "            SELECT name AS \"Name of Plane\", color AS \"Color of Planet\"\n",
    "            FROM planets\n",
    "            WHERE color LIKE \"%blue%\"\n",
    "            \"\"\"\n",
    "pd.read_sql_query(the_color_blue,con)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 9. Select the count of planets that don't have rings as `planets_without_rings`\n",
    "\n",
    "Note: even though the schema states that `rings` is a `BOOLEAN` and the example table shows values `TRUE` and `FALSE`, SQLite does not actually support booleans natively. From the [documentation](https://www.sqlite.org/datatype3.html#boolean_datatype):\n",
    "\n",
    "> SQLite does not have a separate Boolean storage class. Instead, Boolean values are stored as integers 0 (false) and 1 (true)."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "Number of planets without Rings",
         "rawType": "int64",
         "type": "integer"
        }
       ],
       "ref": "725c9f18-4031-4cf4-a858-2c4050a80a71",
       "rows": [
        [
         "0",
         "5"
        ]
       ],
       "shape": {
        "columns": 1,
        "rows": 1
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Number of planets without Rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>5</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Number of planets without Rings\n",
       "0                                5"
      ]
     },
     "execution_count": 64,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "planets_without_rings = \"\"\"\n",
    "            SELECT COUNT(*) AS \"Number of planets without Rings\"\n",
    "            FROM planets\n",
    "            WHERE rings = False\n",
    "            \"\"\"\n",
    "pd.read_sql_query(planets_without_rings,con)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 10. Select the name of all planets, along with a value `has_rings` that returns \"Yes\" if the planet does have rings, and \"No\" if it does not"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 78,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.microsoft.datawrangler.viewer.v0+json": {
       "columns": [
        {
         "name": "index",
         "rawType": "int64",
         "type": "integer"
        },
        {
         "name": "Name of planet",
         "rawType": "object",
         "type": "string"
        },
        {
         "name": "has_rings",
         "rawType": "object",
         "type": "string"
        }
       ],
       "ref": "09346cb2-88b3-4144-9ba3-774290e0d737",
       "rows": [
        [
         "0",
         "Mercury",
         "No"
        ],
        [
         "1",
         "Venus",
         "No"
        ],
        [
         "2",
         "Earth",
         "No"
        ],
        [
         "3",
         "Mars",
         "No"
        ],
        [
         "4",
         "Jupiter",
         "No"
        ],
        [
         "5",
         "Saturn",
         "Yes"
        ],
        [
         "6",
         "Uranus",
         "Yes"
        ],
        [
         "7",
         "Neptune",
         "Yes"
        ]
       ],
       "shape": {
        "columns": 2,
        "rows": 8
       }
      },
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Name of planet</th>\n",
       "      <th>has_rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Mercury</td>\n",
       "      <td>No</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Venus</td>\n",
       "      <td>No</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Earth</td>\n",
       "      <td>No</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Mars</td>\n",
       "      <td>No</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Jupiter</td>\n",
       "      <td>No</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Saturn</td>\n",
       "      <td>Yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Uranus</td>\n",
       "      <td>Yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>Neptune</td>\n",
       "      <td>Yes</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  Name of planet has_rings\n",
       "0        Mercury        No\n",
       "1          Venus        No\n",
       "2          Earth        No\n",
       "3           Mars        No\n",
       "4        Jupiter        No\n",
       "5         Saturn       Yes\n",
       "6         Uranus       Yes\n",
       "7        Neptune       Yes"
      ]
     },
     "execution_count": 78,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "planets_rings = \"\"\"\n",
    "            SELECT Name AS \"Name of planet\",\n",
    "              CASE \n",
    "                WHEN rings = TRUE then \"Yes\"\n",
    "                ELSE \"No\"\n",
    "              END AS has_rings\n",
    "            FROM planets\n",
    "            \"\"\"\n",
    "pd.read_sql_query(planets_rings,con)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Summary"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Congratulations! NASA is one step closer to embarking upon its mission to Mars. In this lab, You practiced writing `SELECT` statements that query a single table to get specific information. You also used other clauses and specified column names to cherry-pick the data we wanted to retrieve. "
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "learn-env",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.13.7"
  },
  "toc": {
   "base_numbering": 1,
   "nav_menu": {},
   "number_sections": true,
   "sideBar": true,
   "skip_h1_title": false,
   "title_cell": "Table of Contents",
   "title_sidebar": "Contents",
   "toc_cell": false,
   "toc_position": {},
   "toc_section_display": true,
   "toc_window_display": false
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
