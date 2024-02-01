# Firefox places.sqlite Database

`places.sqlite` is the SQLite database file that stores history for Mozilla's Firefox browser. It also stores additional data such as favicons, bookmarks, and input history (used for autofill).

### Analysis Value
 - [x] Browser - History
 - [x] Browser - Bookmarks

## Operating System Availability

* Systems with Mozilla Firefox installed

## Artifact Location(s)

* `%UserProfile%\AppData\Roaming\Mozilla\Firefox\Profiles\{FIREFOX_PROFILE}\places.sqlite*`

## Artifact Parsers

* KAPE (Extraction and Parsing)
* [DB Browser for SQLite](https://sqlitebrowser.org/)

## Artifact Interpretation

The following tables are present in this SQLite database:

| Database Table     | Information                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| moz\_bookmarks     | Bookmarks                                                                |
| moz\_favicons      | Favicon store. Provides URLs for each stored favicon.                    |
| moz\_historyvisits | Firefox history                                                          |
| moz\_inputhistory  | Input history for the URL/search bar                                     |
| moz\_places        | Stores URLs and some metadata regarding each URL such as visit frequency |

> [!NOTE]
> In newer versions of Firefox (Firefox 55.0+), the `moz_favicons` table has been moved to its own unique database under the same directory, `favicons.sqlite`.&#x20;

## Firefox Bookmarks

The browser bookmark data for Firefox is stored in the `moz_bookmarks` table of this database. It has the following structure:

<table><thead><tr><th width="176">Field</th><th width="156.33333333333331">Type</th><th>Interpretation</th></tr></thead><tbody><tr><td>fk</td><td>INT</td><td>Points to <code>id</code> in <code>moz_places</code> table, provides the URL for the bookmark.</td></tr><tr><td>title</td><td>LONGVARCHAR</td><td>The user-assigned name for the bookmark</td></tr><tr><td>dateAdded</td><td>INT</td><td>UNIX timestamp indicating when the bookmark was added</td></tr><tr><td>lastModified</td><td>INT</td><td>UNIX timestamp indicating when the bookmark was last modified</td></tr></tbody></table>

## Firefox Favicons

The stored favicons for visited websites are stored either in the `places.sqlite` database for Firefox versions below 55.0, and in its own separate SQLite database `favicons.sqlite` for versions afterwards. The structure is as follows:

<table><thead><tr><th width="172.33333333333331">Field</th><th width="166">Type</th><th>Interpretation</th></tr></thead><tbody><tr><td>icon_url</td><td>TEXT</td><td>The URL of the original favicon</td></tr><tr><td>expire_ms</td><td>INT</td><td>UNIX timestamp representing the expiration time/date for the stored favicon. By default seems to be one week from the last visit. </td></tr><tr><td>data</td><td>BLOB</td><td>The raw favicon data</td></tr></tbody></table>

## Firefox History

The browsing history for a particular Firefox profile can be extracted from the `moz_historyvisits` table. It has the following structure:

<table><thead><tr><th width="170.33333333333331">Field</th><th width="165">Type</th><th>Interpretation</th></tr></thead><tbody><tr><td>place_id</td><td>INT</td><td>Foreign key pointing to an entry in the <code>moz_places</code> table, which can be used to determine the URL that was visited</td></tr><tr><td>visit_date</td><td>INT</td><td>UNIX timestamp representing the time of the visit</td></tr><tr><td>visit_type</td><td>INT</td><td>Enumerated value, the type of visit</td></tr></tbody></table>

The following `visit_type` enumerations can provide additional information regarding how the site was visited:

1. Link followed to visit the URL
2. URL was typed and visited, or selected from an autocomplete result in the search bar
3. URL was visited through a bookmark
4. URL was embedded on another page
5. URL visited through a permanent redirect (HTTP 301)
6. URL visited through a temporary redirect (HTTP 307)
7. URL is a downloaded resource
8. URL was visited in a frame
9. URL was visited because the page was reloaded



