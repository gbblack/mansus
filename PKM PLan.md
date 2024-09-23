## Second Brain Structure

- Use PARA: Projects, Area, Resources, Archives.

- Use Zettelkasten to create links between notes. Create a link whenever you think a connection can be made to another topic or if something is likely to need more clarity in a different note.

- Create Indexes or MOCs to cover a topic in an area.
  i.e. if the area is programming, a topic/index is Rust.

- Use metadata to organise notes and make templates for all kinds of notes.

- Use tags as markers of context on the note, they should be additive not repetitive. i.e. if the note is "Rust - Ownership" the tags could be something like "resource management" or "RAII".

- Tags can also be nested such as "programming/rust" giving more granularity.

- Since tags should be used to denote context and not location, both in the knowledge base and in the topic they will not be helpful to query location, rather they are there to create connections across indices to find connections across disciplines.

- To find the location of a file use the path metadata. The path should be the shortest route from the Home note down the indices into the note itself. The path should also include references to which folder of PARA the note finds itself in.

- Notes should be of different types depending on intended purpose. In this knowledge base there will be Literature Notes, Permanent Notes, Fleeting Notes, Journal Notes, Index Notes and Info Notes. Each of these will have its own template.

> [!info]
> The difference between `Resources` and `Archives` is that of accesibility. 
> 
> Notes placed the `Archives` should be treated as forgotten. They are there as a record of past work and thoughts but are not considered still active or referable. The archive should only include project or work specific notes. Things that were relevant for a short period but belonged to a specific time or thing that is no longer being worked on. An example could be the notes for a presentation that has been delivered or the notes created to write a short essay.
> 
> Literature Notes should be placed in `Resources` as that is what they are, a resource to be referenced.
## Metadata

Each template for every kind of note should include these metadata fields in this order:

- `aliases`: shorthand aliases for the note
- `tags`: tags on the context related in the note
- `path`: path to this note from home, 
- `status`: how complete the note is
- `source`: the source of the information (url, book, podcast)
- `created`: the date the note was created

The `created` and `status` fields will be filled in automatically.
The rest will need to filled in manually each time. Some notes do not require all these fields and others may require more, but for now I think this is a good start.

`created`: The day the note was created written in this format: `YYYY-MM-DD`

`status` should reflect how complete a note is. The status reflects how much work is left to be done on a note until it is considered complete. The stages/values are:

- `Night`: For empty or bare bone notes. What Luhmann called Fleeting notes. Until handwritten notes or scribbles are added to the knowledge base they are considered `Night` notes.
- `Dawn`: These notes have been given a first pass. The writing in this note is mostly unoriginal, containing what was considered most valuable from the resources you're pulling from.
- `Noon`: This note has had a second pass through, where words have been rewritten to fit your own understanding of the topic.
- `Dusk`: This note is complete. It includes all the formatting such as footnotes, tags, links and references. This note can be set aside and considered fully digested. Luhmann would call this a Permanent Note.

`path`: This holds the path to the note's location in the knowledge base, it should always start with home and then include any indexes that contain this note. Gives clarity on what area of interest the note falls into.

`tags`: The tags related to the note. These should be things that are not self evident in the notes title or headings. Tags should lean more towards topics the note could fall into rather than linking to existing notes.

> [!info]- `Resources` vs. `Archives`
> 
> In practice `path` and `tags` may end up being too similar and not have a meaningful difference. My use of both should be reviewed every couple dozen or hundred notes depending on ouput just to see if my intensions match my actions. As this kbowledge base grows the difference, or lack of, in these two properties will present itself.

`source` should contain a link to the original material that leads outside the knowledge base AND a link to the Literature note if it exists.

`aliases`: should be essentially a shorthand of the notes title for easier linking. An example could be a note titled "Artificial Intelligence - A Waste" which would have the alias "AI-waste" or something similar so that when I need to reference this note in a link I can use the shorter alias. This makes creating links easier but also easier for reading back the link when Markdown isn't being formatted.

These fields have been chosen with the intention they will create value over time when querying or analysing the knowledge base as a whole. As it develops more or less fields might be added and this note will be updated. Potential additions could be:

- `updated`, to see when this note was updated. 
- `weight`, for tracking how many times this note was visited/rewritten, denoting its importance.
- `author`: to show a potential preference for a writer or publication.
- `source date`: the date the source was created, displaying the recency of the information.

When adding or removing a field the main thing to consider is its potential use as a vector for spontaneous insight when queried. The existing fields were chose with these values in mind:

- `created`: work over time
- `status`: progress/completion rate
- `path`: navigation within knowledge base
- `tags`: high level context on the topic in the note
- `source`: easy reference to original material
- `aliases`: easier reference to this note, increases fuzziness

## Style Guide

For consistency here are some rules for styling the notes as they are being written.

1. `backticks` should be used whenever a reference to a technical word is used, like the metadata fields above, or something like a keyword in a programming language is used; i.e. `const`, `await`, `use`. This is to help visually differentiate the words that are *of* the topic and words *about* the topic.

2. When trying to create emphasis use *italics* over **bold** lettering as there is more of a difference for the former at first glance.

3. For Headings the title should be the only `H1` heading. The next heading size should be `H3` and then `H5`. If you nedd to go one level further use **Bold** lettering for a fourth heading. If that is still not enough add an `H2` before the `H3` but make sure there is a least a sentence or line break before the `H3` heading.
   
4. When creating a list under a paragraph block use a `:` and then leave an empty line between them:

   - like this.
   - it makes the overall block less cramped while the `:` denotes the connection between them.
   - Each line in a list should also end with a `.`.
   - The same rules apply for numbered and nested lists.

5. When writing lists where each entry is more than one or two lines, keep and empty line between each entry to maintain readability. Otherwise the blocks get too crowded.

6. If the list is full of short one lines however, keep them close together.

7. When using units after a measurement do NOT leave a space between the value and the unit: 300g.
   
8. Create links often and quickly, you can always review later and tidy up duplicates or connect to an existing note. The creation of links should work like a record of your interest without detracting focus from the current topic. When I am writing my note and think "oh I need to know more about that" or "that's cool I should check it out", create the link around the relevant words and move on. These empty links can be a great place to start on new but related avenues of thought when stuck on a topic.

9. Hold `shift` when entering a new line to keep it in line with your indented block.

10. Info blocks should be used when needing to clarify a point that has been just made or as a callout that is of the note itself rather than what it discusses. They should be used infrequently, only applied when the contents within would break from the flow of the text around it. Think of them almost like a web pop up that captures your attention and distracts from your focus. They can be made foldable by adding a `-` or `+` after the tag.

11. When using a direct quote from a basic block quote to denote its alternate source.

12. When writing a line break use this format: `---`.

13. Titles should be written where the first letter of every word starts with a capital, i.e. "Here Is An Example Of A Title"

## Note Taking 

In the case of Literature notes, where you're not trying to translate the author's understanding into your own these steps are simplified. You still pass through all four but the first pass might include everything thing you think interesting from the source, while the second trims it down to its most relevant pieces.The polishing required here is minimal, just a reference to the original and the appropriate metadata.
