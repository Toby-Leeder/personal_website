---
layout: single
title: "Family Tree"
permalink: /family/
author_profile: true
classes: wide
---

<div class="family-tree-wrapper" id="tree-wrapper">
  
  <div class="zoom-controls">
    <button onclick="window.zoomTree(0.1)" title="Zoom In">+</button>
    <button onclick="window.zoomTree(-0.1)" title="Zoom Out">-</button>
    <button onclick="window.fitTreeToScreen()" title="Fit">Fit</button>
    <button onclick="window.resetZoom()" title="1:1">1:1</button>
    <button onclick="window.toggleFullscreen()" title="Fullscreen" style="font-weight:normal">⛶</button>
  </div>

  <div class="family-tree-inner" id="tree-inner">
    <div class="family-tree" id="tree-root"></div>
  </div>

</div>

<style>
  /* ---------------------------------------------------------
     ZOOM CONTROLS
     --------------------------------------------------------- */
  .zoom-controls {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background: white;
    padding: 5px;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    z-index: 2147483647; /* Maximum Z-Index to stay on top */
    display: flex;
    gap: 5px;
  }

  .zoom-controls button {
    width: 40px;
    height: 40px;
    border: 1px solid #ddd;
    background: #fff;
    border-radius: 4px;
    font-size: 14px;
    cursor: pointer;
    font-weight: bold;
    color: #333;
    transition: background 0.2s;
  }

  .zoom-controls button:hover {
    background: #f0f0f0;
  }

  /* ---------------------------------------------------------
     LAYOUT & SCROLLING STYLES
     --------------------------------------------------------- */
  .family-tree-wrapper {
    /* BREAKOUT: Force full viewport width inside Jekyll */
    width: 100vw;
    position: relative;
    left: 50%;
    right: 50%;
    margin-left: -50vw;
    margin-right: -50vw;
    
    /* Default View */
    height: 85vh; 
    overflow: auto; /* Native scrolling */
    background-color: #fcfcfc;
    border-top: 1px solid #eee;
    border-bottom: 1px solid #eee;
    text-align: center;
  }

  /* NATIVE FULLSCREEN OVERRIDES */
  .family-tree-wrapper:fullscreen {
    background-color: #ffffff; /* Essential: Default is black */
    width: 100vw;
    height: 100vh;
    margin: 0;
    left: 0;
    right: 0;
    overflow: auto;
    padding: 0;
  }

  /* Safari syntax for fullscreen */
  .family-tree-wrapper:-webkit-full-screen {
    background-color: #ffffff;
    width: 100%;
    height: 100%;
    margin: 0;
    left: 0;
    overflow: auto;
  }

  .family-tree-inner {
    /* Ensure content determines width so left side isn't cut off */
    width: max-content; 
    min-width: 100%; 
    padding: 100px 50px;
    box-sizing: border-box;
    transform-origin: top center;
    transition: transform 0.2s ease-out;
    margin: 0 auto;
  }

  /* ---------------------------------------------------------
     TREE LINES & STRUCTURE
     --------------------------------------------------------- */
  .family-tree {
    display: inline-block;
    text-align: center;
  }

  .family-tree ul {
    padding-top: 40px; 
    position: relative;
    padding-left: 0;
    white-space: nowrap; 
    display: flex; 
    justify-content: center;
  }

  .family-tree ul::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    border-left: 2px solid #ccc;
    width: 0;
    height: 40px; 
  }

  .family-tree li {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    padding: 40px 10px 0 10px; 
    box-sizing: border-box;
  }

  .family-tree li::before,
  .family-tree li::after {
    content: '';
    position: absolute;
    top: 0;
    border-top: 2px solid #ccc;
    width: 50%;
    height: 40px; 
  }

  .family-tree li::before { right: 50%; }
  .family-tree li::after { left: 50%; border-left: 2px solid #ccc; }

  .family-tree li:only-child::before,
  .family-tree li:only-child::after { display: none; }
  .family-tree li:only-child { padding-top: 0; }

  .family-tree li:first-child::before,
  .family-tree li:last-child::after { border: 0 none; }

  .family-tree li:last-child::before {
    border-right: 2px solid #ccc;
    border-radius: 0;
  }
  .family-tree li:first-child::after { border-radius: 0; }

  /* ---------------------------------------------------------
     CARD STYLES
     --------------------------------------------------------- */
  .family-tree .person-card {
    background: #fff;
    border-radius: 8px;
    padding: 10px;
    border: 1px solid #ddd;
    min-width: 140px; 
    max-width: 180px; 
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.08);
    text-align: left;
    white-space: normal;
    z-index: 2;
  }

  .family-tree .person-name {
    font-weight: 700;
    font-size: 0.9rem;
    margin-bottom: 4px;
    color: #333;
    text-align: center;
    line-height: 1.2;
  }

  .family-tree .person-details {
    font-size: 0.75rem;
    color: #666;
    line-height: 1.35;
    margin-bottom: 6px;
    border-bottom: 1px solid #f0f0f0;
    padding-bottom: 6px;
  }

  .family-tree .spouses {
    background-color: #f8f9fa;
    border-radius: 4px;
    padding: 6px;
    font-size: 0.75rem;
  }

  .family-tree .spouse-block {
    margin-bottom: 6px;
    padding-bottom: 4px;
    border-bottom: 1px dashed #e0e0e0;
  }
  .family-tree .spouse-block:last-child {
    margin-bottom: 0;
    padding-bottom: 0;
    border-bottom: none;
  }

  .family-tree .spouse-label {
    font-weight: 600;
    font-size: 0.7rem;
    color: #999;
    text-transform: uppercase;
    display: block;
  }

  .family-tree .spouse-name {
    font-weight: 600;
    color: #444;
  }

  @media (max-width: 600px) {
    .family-tree-wrapper { height: 70vh; }
  }
</style>

<script>
document.addEventListener("DOMContentLoaded", function() {

  /* ---------------------------------------------------------
     DATA STRUCTURE
     --------------------------------------------------------- */
  const familyRoot = {
    name: "Moshe Klonimus (Kalman, Solomon) Gescheit",
    details: "b: Jan 24, 1812 Balassagyarmat, Hungary · d: Jan 4, 1882",
    spouses: [
      {
        name: "Berta Erzsebet Weiszberger",
        children: [
          {
            name: "Chaim (Jacab) Gescheit",
            details: "b: Aug 17, 1841 Balassagyarmat · d: Sep 18, 1901 Putnok",
            spouses: [
              {
                name: "Lina (Krassil) Phillipp",
                details: "b: 1840 · d: 1936 Putnok · m: Mar 29, 1864 Vac",
                children: [
                  {
                    name: "Jozsef Gescheit",
                    details: "b: Aug 31, 1869 Putnok · d: Jun 15, 1944 Auschwitz",
                    spouses: [
                      {
                        name: "Paula Barok",
                        details: "b: 1873 Salgotarjan · d: Jun 15, 1944 Auschwitz",
                        children: [
                          { name: "Etel Gescheit", details: "b: Jun 12, 1902 · d: Aug 19, 1902" },
                          {
                            name: "Margit Gescheit",
                            details: "b: 1896 · d: Jun 1944 Auschwitz",
                            spouses: [
                              {
                                name: "Frigyes Ullmann",
                                details: "b: 1895 · d: May 1945 Dachau · m: 1923",
                                children: [
                                  {
                                    name: "Sandor Ullmann",
                                    details: "b: Jan 28, 1925 · d: Oct 11, 1994",
                                    spouses: [
                                      {
                                        name: "Eva Faye Schrage",
                                        label: "1st Spouse",
                                        details: "b: 1939 Detroit · m: 1980",
                                        children: [
                                          { name: "Steven Kleiff", details: "b: 1967" },
                                          { name: "Marcy Kleiff", details: "b: Omaha" }
                                        ]
                                      },
                                      {
                                        name: "Hanica Cohn",
                                        label: "2nd Spouse",
                                        details: "b: 1936 Romania · d: 1978 MI · m: 1962",
                                        children: [
                                          {
                                            name: "Sabrina Mauskopf Ullmann",
                                            details: "b: 1955",
                                            spouses: [{
                                              name: "Daniel Matthews",
                                              details: "b: 1948 · m: Portland OR",
                                              children: [
                                                { name: "Gabriel Matthews", details: "b: 1991" },
                                                { name: "Margot Chiara Alexandra Mathews", details: "b: 1995" }
                                              ]
                                            }]
                                          }
                                        ]
                                      },
                                      {
                                        name: "Irene Steinberger",
                                        label: "3rd Spouse",
                                        details: "b: 1927 · d: 1959 Windsor · m: 1948",
                                        children: [
                                          {
                                            name: "Dorothy Margaret Ullmann",
                                            details: "b: 1953 Windsor",
                                            spouses: [{
                                              name: "Stephen Weil",
                                              details: "b: 1952 · m: Boston",
                                              children: [
                                                { name: "Lindsay Sarah Weil", details: "b: Boston" },
                                                { name: "Savannah Weil", details: "b: Boston" }
                                              ]
                                            }]
                                          },
                                          {
                                            name: "Stewart Bruce Ullmann",
                                            details: "b: 1951 Windsor",
                                            spouses: [{
                                              name: "Susan Theresa Engle",
                                              details: "b: 1958 GA · m: 1977 MI",
                                              children: [
                                                { name: "Sarah Eve Ullmann", details: "b: 1980" },
                                                { name: "Noah John-David Ullmann", details: "b: 1981" },
                                                { name: "Carol Irene Ullmann", details: "b: 1978" }
                                              ]
                                            }]
                                          }
                                        ]
                                      }
                                    ]
                                  },
                                  { name: "Dezso Ullmann", details: "b: 1926 · d: Auschwitz" }
                                ]
                              }
                            ]
                          },
                          {
                            name: "Olga (Pici) Gescheit",
                            details: "b: Aug 23, 1913 Salgotarjan",
                            spouses: [
                              { name: "Maurice Blau", details: "m: 1966 NY" },
                              { name: "Alex Gelberman (Rabbi)", details: "m: 1963 NC" },
                              { name: "Martin Sunshine", details: "m: 1952 NY" },
                              { name: "Miklos Itzkovitz", details: "m: 1937 Budapest" }
                            ]
                          },
                          {
                            name: "Samu Gescheit",
                            details: "b: 1898 · d: May 1944 Linstz",
                            spouses: [{
                              name: "Elza Krausz",
                              details: "b: 1903 · d: 1944 Auschwitz · m: 1923",
                              children: [
                                {
                                  name: "Sari (Sarah) Gescheit",
                                  details: "b: 1924 Budapest",
                                  spouses: [{
                                    name: "Deszo (Menachem) Davidovic",
                                    details: "b: 1910 · d: Israel · m: 1944",
                                    children: [
                                      {
                                        name: "Miriam Davidovic",
                                        details: "b: 1946 Czechoslovakia",
                                        spouses: [{
                                          name: "Mickey Kantor",
                                          details: "b: 1936 Tel Aviv",
                                          children: [
                                            {
                                              name: "Lynn Kantor",
                                              details: "b: 1972 Vienna",
                                              spouses: [{
                                                name: "Michael Herrick",
                                                children: [
                                                  { name: "Natalie" },
                                                  { name: "Daniel" },
                                                  { name: "Benjamin" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Ariella Kantor",
                                              details: "b: 1974 MI",
                                              spouses: [{
                                                name: "Eyal Leeder",
                                                children: [
                                                  { name: "Aiden" },
                                                  { name: "Toby" },
                                                  { name: "Bailey" }
                                                ]
                                              }]
                                            }
                                          ]
                                        }]
                                      },
                                      {
                                        name: "Yoram Davidovic",
                                        details: "b: 1950 Tel Aviv",
                                        spouses: [{
                                          name: "Noga (Davidovic)",
                                          details: "b: 1958",
                                          children: [
                                            { name: "Nadav Davidovic", details: "b: 1980" },
                                            { name: "Dana Davidovic", details: "b: 1984" }
                                          ]
                                        }]
                                      }
                                    ]
                                  }]
                                },
                                {
                                  name: "Amram (Karcsi) Gescheit",
                                  details: "b: 1928 Budapest",
                                  spouses: [{
                                    name: "Eva Hoffman",
                                    details: "b: 1930 · m: 1948 Italy",
                                    children: [
                                      {
                                        name: "Esther Gescheit",
                                        details: "b: 1950 Israel",
                                        spouses: [{
                                          name: "Reudor Roichman",
                                          details: "m: 1972 Israel",
                                          children: [
                                            { name: "Nama Roichman", details: "b: 1973" },
                                            { name: "Royi Roichman", details: "b: 1978" },
                                            { name: "Yaara Roichman", details: "b: 1982" }
                                          ]
                                        }]
                                      },
                                      {
                                        name: "Irit Gescheit",
                                        details: "b: 1954",
                                        spouses: [{
                                          name: "Eliazer Gilad",
                                          details: "b: 1954 · m: 1976",
                                          children: [
                                            { name: "Erez Gilad", details: "b: 1984" },
                                            { name: "Adva Gilad", details: "b: 1980" },
                                            { name: "Elad Gilad", details: "b: 1986" }
                                          ]
                                        }]
                                      }
                                    ]
                                  }]
                                },
                                {
                                  name: "Eva Gescheit",
                                  details: "b: Oct 1925",
                                  spouses: [{
                                    name: "Kornel Mermelstein",
                                    details: "m: 1949 Tel Aviv",
                                    children: [
                                      {
                                        name: "Moshe (Mermelstein) Mor",
                                        spouses: [{
                                          name: "Esther (Mor)",
                                          children: [
                                            { name: "Gil Mor", details: "b: 1980" },
                                            { name: "Ran Mor", details: "b: 1982" },
                                            { name: "Noam Mor", details: "b: 1987" }
                                          ]
                                        }]
                                      }
                                    ]
                                  }]
                                },
                                { name: "Tibor Gescheit", details: "b: 1930 · d: 1944 Auschwitz" }
                              ]
                            }]
                          },
                          {
                            name: "Kis Margit Gescheit",
                            details: "b: 1896 · d: 1981 Israel",
                            spouses: [{
                              name: "Isidor Eppler (Rabbi)",
                              details: "d: Concentration camp",
                              children: [
                                {
                                  name: "Esther Eppler",
                                  spouses: [{
                                    name: "Miklos Frankfurter",
                                    children: [
                                      { name: "Ethan Frankfurter" },
                                      { name: "Itzhak Frankfurter" }
                                    ]
                                  }]
                                },
                                { name: "Sandor Eppler", details: "b: 1927 · d: Concentration camp" }
                              ]
                            }]
                          },
                          {
                            name: "Kalman Gescheit",
                            details: "b: 1900 Salgotarjan · d: 1990 Melbourne",
                            spouses: [{
                              name: "Cilike Biron",
                              details: "b: 1901 · d: 1996 Melbourne · m: 1920",
                              children: [
                                {
                                  name: "Sanyi Gescheit",
                                  details: "b: 1921 · d: 1995 Melbourne",
                                  spouses: [{
                                    name: "Erzsi (Gescheit)",
                                    details: "b: 1926 · d: 1989",
                                    children: [
                                      {
                                        name: "Ronald Gescheit",
                                        details: "b: 1957 Melbourne",
                                        spouses: [{
                                          name: "Bonita (Bonnie) Kagan",
                                          children: [
                                            {
                                              name: "Jordan Kagan/ Gescheit",
                                              details: "b: 1986",
                                              spouses: [{
                                                name: "Melissa Gocs",
                                                details: "b: 1988",
                                                children: [
                                                  { name: "Ava Kagan Gescheit", details: "b: 2018" },
                                                  { name: "Oscar Kagan Gescheit", details: "b: 2020" },
                                                  { name: "Millie Kagan Gescheit", details: "b: 2023" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Elliot Kagan Gescheit",
                                              details: "b: 1988",
                                              spouses: [{ name: "Jessica Dadon", details: "b: 1992" }]
                                            },
                                            { name: "Perry Kagan Gescheit", details: "b: 1993" }
                                          ]
                                        }]
                                      },
                                      {
                                        name: "Susan Gescheit",
                                        details: "b: 1946 · d: 2006 Melbourne",
                                        spouses: [{
                                          name: "Steven Tusak",
                                          children: [
                                            {
                                              name: "Paul Tusak",
                                              details: "b: 1975",
                                              spouses: [{
                                                name: "Ada (Tusak)",
                                                details: "b: 1976",
                                                children: [
                                                  { name: "Esmee Tusak", details: "b: 2002" },
                                                  { name: "Corey Tusak", details: "b: 2004" },
                                                  { name: "Tifferet Tusak", details: "b: 2009" },
                                                  { name: "Samara Tusak", details: "b: 2011" },
                                                  { name: "Zahara Tusak", details: "b: 2014" }
                                                ]
                                              }]
                                            }
                                          ]
                                        }]
                                      }
                                    ]
                                  }]
                                },
                                {
                                  name: "Laci (Laszlo) Gescheit",
                                  details: "b: 1923 · d: 2021 Melbourne",
                                  spouses: [{
                                    name: "Lilly Friedmann",
                                    details: "b: 1926 · d: 2022 Melbourne · m: 1945",
                                    children: [
                                      {
                                        name: "Clairie Marilyn Gescheit",
                                        details: "b: 1955",
                                        spouses: [{ name: "Len Faigen", details: "b: 1948" }]
                                      },
                                      {
                                        name: "George Gescheit",
                                        details: "b: 1947",
                                        spouses: [
                                          {
                                            name: "Nikki Loffler",
                                            label: "1st Spouse",
                                            details: "b: 1949 · m: 1970",
                                            children: [
                                              {
                                                name: "Adam Gescheit",
                                                details: "b: 1974",
                                                spouses: [{
                                                  name: "Simone Ray (Rabinowitz)",
                                                  details: "b: 1975 · m: 2011",
                                                  children: [
                                                    { name: "Jesse Gescheit", details: "b: 2011" },
                                                    { name: "Lee Gescheit", details: "b: 2014" }
                                                  ]
                                                }]
                                              }
                                            ]
                                          },
                                          {
                                            name: "Tersa Ifrah",
                                            label: "2nd Spouse",
                                            details: "b: 1950 Morocco · m: 1986 LA",
                                            children: [
                                              { name: "Jasmine Alexa Gescheit", details: "b: 1990" },
                                              { name: "Danielle Tamara Gescheit", details: "b: 1990" }
                                            ]
                                          }
                                        ]
                                      }
                                    ]
                                  }]
                                },
                                {
                                  name: "Imre Gescheit",
                                  details: "b: 1924 · d: 2006 Melbourne",
                                  spouses: [{
                                    name: "Ica Schwarcz",
                                    details: "b: 1926 · d: 2019 · m: 1952",
                                    children: [
                                      {
                                        name: "Robert Joseph Gescheit",
                                        details: "b: 1953",
                                        spouses: [{
                                          name: "Jayne Rose Levinson",
                                          details: "b: 1956 · m: 1978",
                                          children: [
                                            {
                                              name: "Bonnie Rachel Gescheit",
                                              details: "b: 1980",
                                              spouses: [{
                                                name: "Arthur Ickowicz",
                                                details: "b: 1977",
                                                children: [
                                                  { name: "Maya Anne Ickowicz", details: "b: 2010" },
                                                  { name: "Noah Isaac Ickowicz", details: "b: 2013" },
                                                  { name: "Joshua Nechemia Ickowicz", details: "b: 2013" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Jakki Leah Gescheit",
                                              details: "b: 1983",
                                              spouses: [{
                                                name: "Benjamin Drexler",
                                                children: [
                                                  { name: "Toby Drexler", details: "b: 2013" },
                                                  { name: "Talia Drexler", details: "b: 2015" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Eli Gescheit",
                                              details: "b: 1984",
                                              spouses: [{
                                                name: "Keren Johnson",
                                                details: "b: 1984",
                                                children: [
                                                  { name: "Levi Yitzchak Gescheit", details: "b: 2007" },
                                                  { name: "Chana Miriam Gescheit", details: "b: 2010" },
                                                  { name: "Menachem Mendel Gescheit", details: "b: 2013" }
                                                ]
                                              }]
                                            }
                                          ]
                                        }]
                                      },
                                      {
                                        name: "Anita Gescheit",
                                        details: "b: 1955",
                                        spouses: [{
                                          name: "George Olstein",
                                          details: "b: 1953 · m: 1977",
                                          children: [
                                            { name: "Son", details: "b: 1980 · d: 1980" },
                                            { name: "Daughter", details: "b: 1980 · d: 1980" },
                                            {
                                              name: "Bianca Olstein",
                                              details: "b: 1981",
                                              spouses: [{
                                                name: "Andy Jackson",
                                                details: "b: 1982",
                                                children: [
                                                  { name: "Isaiah Jackson", details: "b: 2010" },
                                                  { name: "Ayla Jackson", details: "b: 2013" },
                                                  { name: "Levi Jackson", details: "b: 2015" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Justin Olstein",
                                              details: "b: 1983",
                                              spouses: [{
                                                name: "Ben Pfeiffer",
                                                children: [
                                                  { name: "Ira Tova Olstein Pfeiffer", details: "b: 2024" },
                                                  { name: "Eden Anshul Olstein Pfeiffer", details: "b: 2024" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Rani Olstein",
                                              details: "b: 1988",
                                              spouses: [{
                                                name: "David Janovic",
                                                details: "b: 1984",
                                                children: [
                                                  { name: "Ryder Janovic", details: "b: 2015" },
                                                  { name: "Indy Janovic", details: "b: 2017" },
                                                  { name: "Ollie Janovic", details: "b: 2022" }
                                                ]
                                              }]
                                            }
                                          ]
                                        }]
                                      }
                                    ]
                                  }]
                                },
                                {
                                  name: "Bandi Gescheit",
                                  details: "b: 1926 · d: 2018 Melbourne",
                                  spouses: [{
                                    name: "Ibi (Iboyla, Violet) Strassler",
                                    details: "b: 1930 · d: 1994",
                                    children: [
                                      {
                                        name: "Marika Gescheit",
                                        details: "b: 1953",
                                        spouses: [{
                                          name: "Ted Langer",
                                          details: "Deceased",
                                          children: [
                                            {
                                              name: "Dov Avigdor (Dion) Langer",
                                              details: "b: 1980",
                                              spouses: [{
                                                name: "Ilanit Barzilay",
                                                details: "b: 1980",
                                                children: [
                                                  { name: "Nechama Tehilla Langer", details: "b: 2005" },
                                                  { name: "Aviva Rochel Langer", details: "b: 2008" },
                                                  { name: "Moshe Chaim Langer", details: "b: 2012" },
                                                  { name: "Avner Meir Langer", details: "b: 2017" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Eli (Elazar Eliyahu) Langer",
                                              details: "b: 1982",
                                              spouses: [{
                                                name: "Malka Leah Goldberg",
                                                details: "b: 1982",
                                                children: [
                                                  { name: "Chaya Bracha langer", details: "b: 2012" },
                                                  { name: "Moshe Chaim Langer", details: "b: 2014" },
                                                  { name: "Rivka Chana Langer", details: "b: 2017" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Joshua (Yehoshua Aryeh) Langer",
                                              details: "b: 1988",
                                              spouses: [{
                                                name: "Esther Malky Zamler",
                                                children: [
                                                  { name: "Leah Penina Langer", details: "b: 2024" }
                                                ]
                                              }]
                                            }
                                          ]
                                        }]
                                      },
                                      {
                                        name: "Suzy Gescheit",
                                        details: "b: 1956",
                                        spouses: [{
                                          name: "Stephen Rose",
                                          details: "b: 1954",
                                          children: [
                                            {
                                              name: "George Jonas Eden Rose",
                                              details: "b: 1981",
                                              spouses: [{
                                                name: "Limor Fleischer",
                                                details: "b: 1983",
                                                children: [
                                                  { name: "Arie Indie Rose", details: "b: 2016" },
                                                  { name: "Luca Andy Rose", details: "b: 2019" }
                                                ]
                                              }]
                                            },
                                            {
                                              name: "Jason Ari Rose",
                                              details: "b: 1984",
                                              spouses: [{
                                                name: "Ashleigh Leah Zeimer",
                                                details: "b: 1988",
                                                children: [
                                                  { name: "Dahlia Bluma Violet Rose", details: "b: 2021" },
                                                  { name: "Zara Esther Rose", details: "b: 2023" }
                                                ]
                                              }]
                                            }
                                          ]
                                        }]
                                      }
                                    ]
                                  }]
                                }
                              ]
                            }]
                          },
                          {
                            name: "Bozsi (Elizabeth) Gescheit",
                            details: "d: Concentration camp",
                            spouses: [{
                              name: "Dezso Sauber",
                              children: [
                                { name: "Zsuzsi Sauber" },
                                { name: "Judith Sauber" },
                                { name: "Sanyi Sauber" },
                                { name: "(Boy) Sauber" }
                              ]
                            }]
                          },
                          {
                            name: "Jeno Gescheit",
                            details: "d: Concentration camp",
                            spouses: [{
                              name: "Zelma (Gescheit)",
                              children: [
                                { name: "Liza Gescheit" },
                                { name: "Mimi Gescheit" }
                              ]
                            }]
                          },
                          {
                            name: "Iren Gescheit",
                            details: "d: Concentration camp",
                            spouses: [{
                              name: "Arthur Schwartz",
                              children: [
                                { name: "Ferenc Schwartz" },
                                { name: "Laci Schwartz" },
                                { name: "(Boy) Schwartz" },
                                { name: "(Boy) Schwartz" }
                              ]
                            }]
                          },
                          {
                            name: "Gizella Gescheit",
                            details: "d: Auschwitz",
                            spouses: [{
                              name: "Sandor Weinstock",
                              details: "d: 1979",
                              children: [
                                { name: "Gyorgy Weinstock", details: "d: Auschwitz" }
                              ]
                            }]
                          },
                          {
                            name: "Jolan (Yolanda) Gescheit",
                            details: "d: Concentration camp",
                            spouses: [
                              {
                                name: "Sandor Haber",
                                details: "d: 1936",
                                children: [
                                  { name: "Miklos Haber", details: "d: Concentration camp" }
                                ]
                              },
                              { name: "Karoly Unknown", details: "m: 1940" }
                            ]
                          }
                        ]
                      }
                    ]
                  },
                  {
                    name: "Adolf Abraham Gescheit",
                    details: "b: 1867 Putnok · d: 1930 Calgary",
                    spouses: [{
                      name: "Bertha Brummel",
                      details: "b: 1864 CT · d: 1941 San Francisco",
                      children: [
                        {
                          name: "Ruth Gescheit",
                          details: "b: 1896 IA · d: 1992 SF",
                          spouses: [{
                            name: "Hubert Arthur Graf",
                            details: "b: 1895 Austria · d: 1968 SF",
                            children: [
                              { name: "Gerald Graf", details: "b: 1920 · d: 1944 Normandy" },
                              { name: "Laurence Graf", details: "b: abt 1925" }
                            ]
                          }]
                        }
                      ]
                    }]
                  },
                  {
                    name: "Samu Shmuel (Michael) Gescheit",
                    details: "b: 1871 Putnok · d: 1922",
                    spouses: [{
                      name: "Chaya Braun",
                      children: [
                        {
                          name: "Giza Gescheit",
                          details: "d: HOLOCAUST",
                          spouses: [{
                            name: "Jermi Naulender",
                            children: [
                              {
                                name: "Bozsi Naulender",
                                spouses: [{ name: "Karcsi Berger" }]
                              },
                              {
                                name: "Chaim Naulender",
                                spouses: [{
                                  name: "Ili (Naulender)",
                                  children: [
                                    {
                                      name: "Jermi Naulender",
                                      spouses: [{
                                        name: "Lea (Naulender)",
                                        children: [
                                          { name: "Avri Naulender" },
                                          { name: "Rinat Naulender" },
                                          { name: "Arik Naulender" },
                                          { name: "Aviva Naulender" }
                                        ]
                                      }]
                                    },
                                    {
                                      name: "Yitzchak Naulender",
                                      spouses: [{
                                        name: "Yochevet (Naulender)",
                                        children: [{ name: "Ilan Naulender" }]
                                      }]
                                    },
                                    {
                                      name: "Chaya Naulender",
                                      spouses: [{
                                        name: "Avri Unknown",
                                        children: [
                                          { name: "Szara Unknown" },
                                          { name: "Rachael Unknown" },
                                          { name: "Orit Unknown" }
                                        ]
                                      }]
                                    },
                                    {
                                      name: "Yehuda Naulender",
                                      spouses: [{
                                        name: "Eti (Naulender)",
                                        children: [
                                          { name: "Romi Naulender" },
                                          { name: "Orit Naulender" },
                                          { name: "Zvi Naulender" }
                                        ]
                                      }]
                                    }
                                  ]
                                }]
                              },
                              {
                                name: "Lili Naulender",
                                spouses: [{
                                  name: "Sanyi Roth",
                                  children: [
                                    {
                                      name: "Yutka Roth",
                                      spouses: [{
                                        name: "Yoszi (Unknown)",
                                        children: [
                                          { name: "Chaya (Unknown)" },
                                          { name: "Rana (Unknown)" },
                                          { name: "Sarah (Unknown)" }
                                        ]
                                      }]
                                    },
                                    {
                                      name: "Dvori Roth",
                                      spouses: [{
                                        name: "Tal (Unknown)",
                                        children: [
                                          { name: "Toner (Unknown)" },
                                          { name: "Yanin (Unknown)" },
                                          { name: "Rotem (Unknown)" }
                                        ]
                                      }]
                                    }
                                  ]
                                }]
                              },
                              { name: "Ervin Naulender" },
                              { name: "Manci Naulender" }
                            ]
                          }]
                        },
                        {
                          name: "Feri Gescheit",
                          spouses: [{
                            name: "Ilus Braun",
                            children: [
                              {
                                name: "Shmuel Navon",
                                spouses: [{
                                  name: "Agi Rety",
                                  children: [
                                    { name: "Avri Navon" },
                                    { name: "Schula Navon" },
                                    { name: "Cipi Navon" },
                                    { name: "Jehuda Navon" },
                                    { name: "Rachel Navon" }
                                  ]
                                }]
                              },
                              {
                                name: "Lili Gescheit",
                                spouses: [{
                                  name: "Unknown Grosberg",
                                  children: [
                                    { name: "Sos Grosberg" },
                                    { name: "Yehizkael Grosberg" },
                                    { name: "Judit Grosberg" },
                                    { name: "Avki Grosberg" }
                                  ]
                                }]
                              }
                            ]
                          }]
                        },
                        {
                          name: "Ilonka Gescheit",
                          details: "d: HOLOCAUST",
                          spouses: [{
                            name: "Moritz Hirsch",
                            children: [
                              { name: "Boy Hirsch" },
                              { name: "Girl Hirsch" },
                              { name: "Girl Hirsch" }
                            ]
                          }]
                        },
                        {
                          name: "Gyula (Yaakov) Gescheit",
                          spouses: [{
                            name: "Kati Braun",
                            children: [
                              {
                                name: "Shmuel (Mitza) Gescheit",
                                spouses: [{
                                  name: "Ibi Steinfeld",
                                  children: [
                                    {
                                      name: "Yehuda Gescheit",
                                      spouses: [{
                                        name: "Dorit (Gescheit)",
                                        children: [
                                          { name: "Iddo Gescheit" },
                                          { name: "Ilay Gescheit" },
                                          { name: "Jonathan Gescheit", details: "b: 1991" }
                                        ]
                                      }]
                                    },
                                    {
                                      name: "Ilan Gescheit",
                                      spouses: [{ name: "Tali (Unknown)" }]
                                    }
                                  ]
                                }]
                              },
                              { name: "Moshe Gescheit", details: "d: HOLOCAUST" },
                              { name: "Chaya Szara Gescheit", details: "d: HOLOCAUST" }
                            ]
                          }]
                        },
                        {
                          name: "Lenke Gescheit",
                          spouses: [{
                            name: "M. Roth",
                            children: [
                              { name: "Klari Gescheit", details: "d: HOLOCAUST" },
                              { name: "Imre Gescheit", details: "d: HOLOCAUST" }
                            ]
                          }]
                        },
                        {
                          name: "Jakob Gescheit",
                          details: "d: HOLOCAUST",
                          spouses: [{
                            name: "Bela Grunfeld",
                            details: "d: HOLOCAUST",
                            children: [
                              { name: "Shmuel Gescheit", details: "d: HOLOCAUST" },
                              { name: "Moshe Gescheit", details: "d: HOLOCAUST" },
                              { name: "Marta Gescheit", details: "d: HOLOCAUST" }
                            ]
                          }]
                        },
                        {
                          name: "Jeno Gescheit",
                          details: "d: HOLOCAUST",
                          spouses: [{ name: "Sari (Gescheit)" }]
                        },
                        {
                          name: "Sanyi Gescheit",
                          details: "d: after WW2",
                          spouses: [{
                            name: "Rene (Gescheit)",
                            children: [
                              { name: "Boy Gescheit" },
                              { name: "Boy Gescheit" },
                              { name: "Boy Gescheit" }
                            ]
                          }]
                        }
                      ]
                    }]
                  },
                  { name: "Belli Gescheit", details: "b: 1873 Putnok" },
                  { name: "Hirsch Gescheit", details: "b: 1875 Putnok" },
                  {
                    name: "Helen (Lena) Gescheit",
                    details: "b: 1876 Putnok · d: Cleveland",
                    spouses: [{
                      name: "Sigmund Hayes",
                      children: [
                        {
                          name: "Mildred Hayes",
                          details: "b: 1902",
                          spouses: [{ name: "Leo Schultz" }]
                        }
                      ]
                    }]
                  },
                  { name: "William (Vilmos) Gescheit", details: "b: 1878 Putnok · d: 1940 Canada" },
                  { name: "Ignatz (Ignac) Gescheit", details: "b: 1880 Putnok · d: 1950 Canada" }
                ]
              }
            ]
          },
          {
            name: "Netti Gescheit",
            spouses: [{ name: "Miksa Flamm" }]
          },
          {
            name: "Herman Gescheit",
            details: "b: 1838 · d: 1901 Putnok",
            spouses: [{
              name: "Maria Kellner",
              details: "b: 1841 · d: 1927 Putnok",
              children: [
                { name: "Bela Gescheit", details: "b: 1861 · d: 1944" },
                { name: "Joseph Gescheit", details: "b: 1858 · d: 1929" },
                { name: "Bertha Gescheit", details: "b: 1859 · d: 1944" },
                { name: "Isadore Gescheit", details: "b: 1862 · d: 1944" },
                { name: "Zoltan Otto Gescheit", details: "b: 1870 · d: 1944" },
                { name: "Mila Gescheit", details: "b: 1874 · d: 1944" },
                { name: "Etelka Gescheit", details: "b: 1875 · d: 1950" },
                { name: "Gizella Gitel Gescheit", details: "b: 1879 · d: 1906" }
              ]
            }]
          },
          {
            name: "Rosa Gescheit",
            details: "b: abt 1840 · d: 1933 Balassagyarmat",
            spouses: [{
              name: "Armin Gescheit",
              details: "b: 1834 · d: 1910",
              children: [
                { name: "Elisabeth Gescheit", details: "b: 1861" },
                { name: "Judugabunan Gescheit", details: "b: 1863" },
                {
                  name: "Samuel Shmuel Gescheit",
                  details: "b: 1864 · d: 1908",
                  spouses: [{
                    name: "Francisca (Fani) Fleischman",
                    details: "b: 1873 · d: 1930 Yugoslavia",
                    children: [
                      {
                        name: "Bela Gescheit/Lowy",
                        details: "b: 1904 · d: 1944 Mauthausen",
                        spouses: [{
                          name: "Veronika (Vera) Sar",
                          details: "b: 1911 · d: 2006",
                          children: [
                            {
                              name: "Peter Lowy/Halevi",
                              details: "b: 1934 Zagreb",
                              spouses: [{
                                name: "Bracha Shaull",
                                details: "b: 1938 Jerusalem",
                                children: [
                                  {
                                    name: "Yael Halevi",
                                    details: "b: 1965 Haifa",
                                    spouses: [{
                                      name: "Daniel Wise",
                                      children: [
                                        { name: "Talia Wise", details: "b: 1995" },
                                        { name: "Steven Joseph Wise", details: "b: 1998" },
                                        { name: "Ariye Wise", details: "b: 2003" },
                                        { name: "Yonatan Wise", details: "b: 2006" }
                                      ]
                                    }]
                                  },
                                  {
                                    name: "Leor Edward Halevi",
                                    details: "b: 1971 Montreal",
                                    spouses: [{
                                      name: "Lauren Clay",
                                      children: [
                                        { name: "Joshua Halevi", details: "b: 2003" },
                                        { name: "Nathaniel Halevi", details: "b: 2006" },
                                        { name: "Naomi Halevi", details: "b: 2010" }
                                      ]
                                    }]
                                  },
                                  {
                                    name: "Dalit Leonor Halevi",
                                    details: "b: 1975 Mexico",
                                    spouses: [{
                                      name: "Janier Nunez",
                                      children: [
                                        { name: "Asher Nunez Halevi", details: "b: 2004" },
                                        { name: "Hanna Nunez Halevi", details: "b: 2006" }
                                      ]
                                    }]
                                  }
                                ]
                              }]
                            }
                          ]
                        }]
                      },
                      {
                        name: "Manci (Margit) Gescheit/Lowy",
                        details: "d: 1944 Auschwitz",
                        spouses: [{
                          name: "Eugen Herzl",
                          children: [
                            {
                              name: "Pali Herzl",
                              details: "d: abt 2004",
                              spouses: [{
                                name: "Lea Lampel",
                                children: [
                                  { name: "Varda Herzl", details: "b: abt 1950" }
                                ]
                              }]
                            }
                          ]
                        }]
                      }
                    ]
                  }]
                },
                { name: "Josef Gescheit", details: "b: 1867" }
              ]
            }]
          },
          {
            name: "Markus Gescheit",
            details: "b: 1847 · d: 1917 Putnok",
            spouses: [{
              name: "Malka Rosnak",
              details: "b: 1853 · d: 1924 Putnok",
              children: [
                { name: "Yosef Gescheit", details: "b: 1879" },
                { name: "Dov Berel Gescheit", details: "b: 1881" }
              ]
            }]
          }
        ]
      },
      {
        name: "Rosal Sarah Weiszberger",
        label: "2nd Spouse",
        details: "d: Aug 31, 1891",
        children: [
          { name: "Rezi Gescheit" },
          { name: "Bernat Gescheit" },
          { name: "Unknown Gescheit" }
        ]
      }
    ]
  };

  /* ---------------------------------------------------------
     RENDERING LOGIC
     --------------------------------------------------------- */
  function createPersonCard(person) {
    const card = document.createElement("div");
    card.className = "person-card";

    const nameEl = document.createElement("div");
    nameEl.className = "person-name";
    nameEl.textContent = person.name;
    card.appendChild(nameEl);

    if (person.details && person.details.trim() !== "") {
      const detailsEl = document.createElement("div");
      detailsEl.className = "person-details";
      detailsEl.textContent = person.details;
      card.appendChild(detailsEl);
    }

    if (person.spouses && person.spouses.length > 0) {
      const spousesWrapper = document.createElement("div");
      spousesWrapper.className = "spouses";

      person.spouses.forEach((spouse, index) => {
        const spouseBlock = document.createElement("div");
        spouseBlock.className = "spouse-block";

        const label = document.createElement("span");
        label.className = "spouse-label";
        if (spouse.label) {
          label.textContent = spouse.label;
        } else {
          label.textContent = (index === 0 && person.spouses.length === 1) ? "Spouse:" : `Spouse ${index + 1}:`;
        }
        spouseBlock.appendChild(label);

        const nameSpan = document.createElement("span");
        nameSpan.className = "spouse-name";
        nameSpan.textContent = spouse.name;
        spouseBlock.appendChild(nameSpan);

        if (spouse.details && spouse.details.trim() !== "") {
            const detailsSpan = document.createElement("div");
            detailsSpan.style.marginTop = "2px";
            detailsSpan.style.color = "#555";
            detailsSpan.textContent = spouse.details;
            spouseBlock.appendChild(detailsSpan);
        }

        spousesWrapper.appendChild(spouseBlock);
      });

      card.appendChild(spousesWrapper);
    }

    return card;
  }

  function renderPersonNode(person) {
    const li = document.createElement("li");
    li.appendChild(createPersonCard(person));

    const allChildren = [];
    if (person.spouses && person.spouses.length > 0) {
      person.spouses.forEach(spouse => {
        if (spouse.children && spouse.children.length > 0) {
          allChildren.push(...spouse.children);
        }
      });
    }

    if (allChildren.length > 0) {
      const ul = document.createElement("ul");
      allChildren.forEach(child => {
        ul.appendChild(renderPersonNode(child));
      });
      li.appendChild(ul);
    }

    return li;
  }

  function renderTree(rootPerson, rootElementId) {
    const container = document.getElementById(rootElementId);
    if (!container) return; // Guard
    container.innerHTML = "";
    const ul = document.createElement("ul");
    ul.appendChild(renderPersonNode(rootPerson));
    container.appendChild(ul);
  }

  // Draw the tree immediately
  renderTree(familyRoot, "tree-root");

  /* ---------------------------------------------------------
     ZOOM / SCROLL LOGIC
     --------------------------------------------------------- */
  let currentScale = 1;
  const treeInner = document.getElementById('tree-inner');
  const treeWrapper = document.getElementById('tree-wrapper');

  function applyZoom() {
    if (!treeInner) return;
    treeInner.style.transform = `scale(${currentScale})`;
  }

  window.zoomTree = function(delta) {
    const newScale = currentScale + delta;
    if (newScale > 0.05 && newScale < 3.0) {
      currentScale = newScale;
      applyZoom();
    }
  };

  window.resetZoom = function() {
    currentScale = 1;
    applyZoom();
  };

  window.fitTreeToScreen = function() {
    if (!treeInner || !treeWrapper) return;
    
    // 1. Reset scale to 1 to measure the real size
    treeInner.style.transform = `scale(1)`;
    
    setTimeout(() => {
      const wrapperWidth = treeWrapper.clientWidth;
      const treeWidth = treeInner.scrollWidth; 
      
      if (treeWidth < 50) { 
        currentScale = 1;
        applyZoom();
        return; 
      }

      // 2. Calculate Fit Scale
      const widthRatio = (wrapperWidth - 60) / treeWidth; 
      let bestScale = Math.min(widthRatio, 1);
      
      if (bestScale < 0.2) bestScale = 0.2; 

      currentScale = bestScale;
      applyZoom();

      // 3. FORCE SCROLL TO CENTER
      // We calculate how much wider the tree is than the wrapper,
      // divide by 2, and that's our scrollLeft value.
      const scrollCenter = (treeWidth - wrapperWidth) / 2;
      
      // Only scroll if positive (if tree is wider than screen)
      if (scrollCenter > 0) {
        treeWrapper.scrollLeft = scrollCenter;
      }

    }, 100);
  };
  
  // Native Fullscreen Toggle
  window.toggleFullscreen = function() {
    const elem = document.getElementById('tree-wrapper');
    
    if (!document.fullscreenElement && !document.webkitFullscreenElement) {
      // Enter Fullscreen
      if (elem.requestFullscreen) {
        elem.requestFullscreen();
      } else if (elem.webkitRequestFullscreen) { /* Safari */
        elem.webkitRequestFullscreen();
      }
    } else {
      // Exit Fullscreen
      if (document.exitFullscreen) {
        document.exitFullscreen();
      } else if (document.webkitExitFullscreen) { /* Safari */
        document.webkitExitFullscreen();
      }
    }
    
    // Re-fit tree after a short delay to account for animation
    setTimeout(window.fitTreeToScreen, 200);
  };

  // Auto-fit and Center on Load
  setTimeout(window.fitTreeToScreen, 250);
});
</script>