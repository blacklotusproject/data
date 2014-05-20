# Black Lotus Project Pricing Data

[Black Lotus Project](http://blacklotusproject.com) (BLP) was a Magic: The Gathering card pricing website that collected prices from the [Magic Online Trading League](http://www.magictraders.com/)'s (MOTL) weekly price guide. The price guide was updated once per day, and so the prices were collected once per day. BLP collected data from 2008-10-28 to 2013-10-22 after which point MOTL stopped updating its price guide.

## Downloading the data

[black-lotus-project-data-2013-10-22.tgz](http://blacklotusproject.com/media/data/black-lotus-project-data-2013-10-22.tgz) (319MB)

The data is available for download from the link above on blacklotusproject.com.

## Data quality

When new sets first went on sale, cards from the new sets would quickly be sold among players and show up on MOTL's price sources. It would sometimes take MOTL weeks to update its card lists with the new sets, which unfortunately led to associating prices from new cards with cards from older sets.

The data here is unchanged from MOTL's lists, which means there are periods where old cards normally worth a few cents have short periods of inaccurate high valuations.

## Understanding the data

The archive consists of 8 CSV files that were dumped from a MySQL database.

* `artists_artist.csv`
* `cards_card.csv`
* `cards_cardset.csv`
* `cards_dataload.csv`
* `cards_setblock.csv`
* `cards_valuepoint.csv`
* `pricelists_pricelist.csv`
* `pricelists_pricelisttype.csv`

### `artists_artist.csv`

`id`: Primary key

`name`: Name of the author

`website`: Known site of the author (sparsely populated by hand)

### `cards_card.csv`

`id`: Primary key

`card_set_id`: Foreign key to `cards_cardset.csv`

`name`: Name of the card

`color`: Color of the card ("Multicolor" if more than one)

`rarity`: One-character rarity of card {"C": "Common", "L": "Land", "M": "Mythic rare", "R": "Rare", "S": "Special (promotional or other)", "U": "Uncommon"}

`artist_id`: Foreign key to `artists_artist.csv`

### `cards_cardset.csv`

`expansion_code`: Primary key; 3-character code for expansion used by [Wizards of the Coast](http://company.wizards.com/) (WotC)

`name`: Name of the card set

`set_block_id`: Foreign key to `cards_setblock.csv`

`motl_abbreviation`: Expansion code used by MOTL, which differs primarily in earlier sets; (Example: "Revised Edition" is identified as "3ED" by WotC but as "RV" by MOTL)

`release_date`: Date expansion was made available for retail purchase

`card_count`: Number of cards in the set

### `cards_dataload.csv`

`id`: Primary key

`date`: Date and time load started

`prices_used_count`: Number of card prices that matched cards in `cards_cards.csv` at the time of the load and were used for graphs on blacklotusproject.com.

`prices_unused_count`: Number of card prices that did not match cards in `cards_cards.csv` at the time of the load and were not used for graphing

### `cards_setblock.csv`

`id`: Primary key

`name`: Name taken from WotC's [Gatherer](http://gatherer.wizards.com) for set block

### `cards_valuepoint.csv`

`id`: Primary key

`card_id`: Foreign key to `cards_cards.csv`

`dataload_id`: Foreign key to `cards_dataload.csv`

`price`: Average card price in USD of prices within 1 standard deviation of `average`

`standard_deviation`: Standard deviation of price set used to calculate `price` in USD

`average`: Mathematical mean of prices in price set in USD

`high`: Highest price in price set in USD

`low`: Lowest price in price set in USD

`change`: Change in price in USD from last valuepoint's price (can be negative)

`raw_number`: Number of prices in price set

`date`: Creation date of valuepoint

`percent_change`: Percent change in price from last valuepoint's price (can be negative)

### `pricelists_pricelist.csv`

`id`: Primary key

`created_at`: Creation date of pricelist

`file`: Absolute path to original pricelist from MOTL; Prepend "http://blacklotusproject.com/media/" to access the original file

### Contributing

If you do any clean up of the data, make interesting finds, or do anything cool at all, open an issue on GitHub to share: [blacklotusproject/data](https://github.com/blacklotusproject/data)

### License

All data is available under a standard MIT License, which is included in the archive itself. Refer to it for full details, but the gist is: do with this data what you like but always cite your source.