---
elm:
  dependencies:
    gicentre/elm-vegalite: latest
---

# Car Sales by Country Average Income in USD

After doing some extensive research on car brands and whether they are considered regular, premium or luxury I have looked up car sales by brand for each country and divided all the brands into the 3 categories, added up all the sales of each category and worked out what percentage of the market each car type takes in a given country. My goal was to answer the question of whether people tend to spend more money on cars if they have a higher income and the type of cars people in different income ranges own. I have used a stacked area chart for this in order to show how the area occupied by each type of car changes with the income. I really liked this design because it helps to predict the various car market share easily by moving along the X axis and checking what the area above the value is.

India: 2150
Russia: 11250
Spain: 30500
Switzerland: 85500
Monaco: 166000
Global Average: 9900

```elm {v}
import VegaLite exposing (..)


carType : Spec
carType =
    let
        data =
            dataFromUrl "http://bntconstruction.co.uk/dataF.csv"

        carColours =
            categoricalDomainMap
                [ ( "Luxury", "rgb(253, 192, 134)" )
                , ( "Premium", "rgb(190, 174, 212)" )
                , ( "Regular", "rgb(127, 201, 127)" )
                ]

        enc =
            encoding
                << position X
                    [ pName "AvgIncome"
                    , pTitle "Income"
                    , pSort [ soCustom (strs [ "2150", "9900", "11250", "30500", "85500", "166000" ]) ]
                    ]
                << position Y
                    [ pName "Percentage"
                    , pAggregate opSum
                    , pTitle "Share%"
                    ]
                << color
                    [ mName "CarType"
                    , mTitle "Car Type"
                    , mScale carColours
                    ]
    in
    toVegaLite [ width 500, height 600, data [], enc [], area [] ]
```

# Car Ownership by Country average Income

I have also researched the number of vehicles per 1000 people in each country and wanted to check if there is a correlation between a country's average income and the number of cars. Therefore i decided to use a bar chart with the bars colored after a Sequential Single-Hue Schemes that becomes darked as the income increases, making it easy to understand that a longer and darker coloured bar signifies that the country's income and the percentage of people who own a car are both high.

```elm {v}
carT2ype : Spec
carT2ype =
    let
        data =
            dataFromUrl "http://bntconstruction.co.uk/dataF.csv"

        enc =
            encoding
                << position Y
                    [ pName "CarOwners"
                    , pTitle "CarOwners%"
                    , pQuant
                    , pSort [ soCustom (strs []) ]
                    , pAxis [ axLabelAngle 0 ]
                    ]
                << position X
                    [ pName "AvgIncome"
                    , pTitle "Income"
                    , pSort [ soCustom (strs [ "2150", "9900", "11250", "30500", "85500", "166000" ]) ]
                    ]
                << color
                    [ mName "AvgIncome"
                    , mQuant
                    , mTitle "Income"
                    , mScale [ scScheme "greens" [] ]
                    ]
    in
    toVegaLite [ width 500, height 600, data [], enc [], bar [] ]
```
