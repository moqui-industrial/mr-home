# IronStone HOME — Initial design

November 1, 2026 — draft for discussion. This proposes the application set, shared foundations and delivery order;
it is not a screen specification or a commitment to build every feature in the first release.

## 1. Purpose

IronStone HOME (Home Organization-Mechanization Ensuite) helps people organize life at home: their shared digital
space, photos/music/videos, ebooks and audiobooks, conversations, projects, schoolwork and homeschooling, chores,
house and vehicles, kitchen, and money.
It should be useful to one person, a family, or another household arrangement, with selected participation by
extended family and friends.

HOME uses the capabilities behind IronStone BOSS, with applications designed around household activities. A meal
plan starts with what to eat this week; cleaning starts with what needs doing; personal finance starts with money
coming in and going out. People should not have to learn purchasing, manufacturing, HR or bookkeeping to do these
things. Commons and Projects can retain much of their breadth, and Maintenance has particularly close parallels.

The same installation can serve **Jones Household** and **David E Jones Consulting** as separate internal
organizations. David can use both HOME and BOSS with one account and appropriate grants. Another household member
can use only HOME. Installing the BOSS dependency must not make its applications visible or accessible to that person.

The experience should be warmer and less formal than BOSS, with readable, approachable screens for children who
can read, write and use a computer or phone. Parents and other responsible adults decide what a child's account can
access and provide the support the child needs. The system supplies understandable permissions and usable tools;
it does not prescribe a parenting method or infer readiness from an age threshold.

### 1.1 Design guideline: child autonomy, enablement and safety

**Enable children to act independently by default, with child safety a high priority.** HOME should help a child
plan, learn, create, contribute to the household and ask for help. It is not a tool for micromanaging or monitoring
children. This guideline applies to the apps, their defaults, notifications, reports and the child variation of Assist.

- **Give the child useful agency.** Within their available capabilities, children can start their own projects,
  organize work, choose reminders, record progress and finish ordinary tasks without adult approval at each step.
- **Make support available.** Help, collaboration and review are easy to request. Required adult review is a visible
  choice for a particular activity or safety need, not the default meaning of a child account.
- **Keep safety boundaries clear and proportionate.** Adults choose access and suitable AI options, explain them
  in terms the child understands and can adjust them as the child's needs and abilities develop. Routine actions
  within those boundaries should work independently.
- **Respect privacy.** Personal notes and AI conversations are not automatically shared with adults. Shared task
  status supports coordination; dashboards monitoring a child's activity, study time or inferred behavior and
  automatic transcript reports are outside the intended experience. A child may use a timer or progress notes for
  their own planning. Necessary security and transaction audit remain separate, access-controlled records with an
  explained purpose.
- **Curate AI for children.** Offer useful, reviewed ways to learn, create and organize, with enforced limits on
  content, sources, data exposure and actions. Safety must not depend solely on a prompt or an adult watching every
  conversation. Non-child accounts retain the ordinary business-user Assist access controls and limits.

## 2. Starting point and reuse strategy

The component currently contains its README, license and [component.xml](../component.xml), which already depends
on `mr-boss`; BOSS depends on `mr-mantle`. Keep that dependency for the initial work. A HOME-only *user experience*
does not initially require a distribution with BOSS physically absent.

The current sources distinguish working foundations from planned capabilities:

| Foundation | What HOME can build on | Qualification |
| --- | --- | --- |
| Commons and Comms | Contacts, calendars, tasks, files/drives, notes/wiki, communication and the shared shell | Substantial implementation exists; use the [implementation record](../../mr-boss/plan/impl-drawers-commons.md) and current sources rather than treating every item in the original design as built. HOME mounts and access rules are new work. |
| Projects | Projects, work items, boards, timelines, milestones, files, discussions, code repositories and optional time tracking | The [Projects implementation](../../mr-boss/plan/impl-projects.md) records P0–P11 built. HOME Projects and the separate School app can reuse its work-item foundation; School's curriculum planning and learner records are new work. |
| Accounting | Banking, payments, reconciliation, budgets, reports and guarded ledger operations | The [Accounting implementation](../../mr-boss/plan/impl-accounting.md) records P0–P21 built. A personal-finance workflow and its posting policy still need design. |
| Maintenance | Existing facilities, assets, work efforts, requests, equipment history and shared screens; a detailed maintenance design | [Maintenance](../../mr-boss/plan/design-maintenance.md) remains a draft, and its app has placeholder screens. Full preventive scheduling and procedure execution are dependencies to build, not finished functionality to rename. |
| Admin and operations | Framework identity, sessions, credentials, monitoring and operational tools; the BOSS Admin design | [Admin](../../mr-boss/plan/design-admin.md) is also a draft with placeholder app screens. Delegated household administration must be implemented over the existing primitives. |
| Sync and collaboration | WebDAV, CalDAV, CardDAV, SFTP, file versions and native collaborative notes | These have [Sync](../../../../docs/interfaces/Sync.md) and [Revision management](../../../../docs/data/Revision-Management.md) guides. Their documented limits still apply. |
| Media storage and delivery | ResourceFacade/ResourceReference, drive access, image previews, git and DbResource versions, content blobs and streamed file responses with byte ranges | [Revision management](../../../../docs/data/Revision-Management.md) and [Git hosting](../../../../docs/interfaces/Git-Hosting.md) document the foundations. The media/ebook libraries, readers/players, personal progress and consistent media access across all four storage backends are new work. |

Reuse in three ways:

1. **Share existing screens when the activity is the same.** File browsing, calendars, messages, task detail and
   project boards should gain a HOME context where needed, keeping one implementation and the same record.
2. **Build household workflows over shared services.** Curriculum planning, meals, chores and the Money register
   need their own vocabulary and flow. Reuse Mantle records and operations where they fit; extend the model where
   they do not.
3. **Put reusable rules in their owning layer.** Mantle owns domain rules and shared data; the framework owns
   protocols and general infrastructure; HOME owns household navigation, presentation, app access and app-specific
   orchestration. Mantle must continue to work without HOME or BOSS group names or screen dependencies.

Do not fork whole BOSS applications, add pass-through services merely to rename existing operations, or require a
household to configure business modules to unlock a home task. These follow the
[application development practices](../../../../docs/dev/Practices-App-Dev.md).

## 3. Proposed initial application set

Use **IronStone HOME** as the suite identity, with short names inside its navigation. Where both suites appear in
the launcher, distinguish HOME Commons from BOSS Commons and HOME Projects from BOSS Projects. Exact branding and
route names are open. Each app has its own authorization gate; users see only the apps available to them.

**Today** is the default household landing page, not another independent application. It brings together the
person's appointments, schoolwork and chores, the household meal plan, and permitted reminders, with links back to
the owning app. A child, a household organizer and a guest get different results from the same scoped providers.
For a child, Today is their own planning space: personal goals and projects sit alongside shared responsibilities,
and the child can arrange their day and reminders. Adults see the shared work they participate in, without a
child-activity dashboard or a notification for every ordinary action.

| Proposed app | Household purpose and initial sections | Reuse from BOSS and its foundations |
| --- | --- | --- |
| **Commons** | Calendar, contacts, files, notes, household knowledge and personal tasks | Most Commons functionality: calendars and sharing, contact books, drives, versions, wiki, attachments, search and Today. Office editing uses the shared integration when available. |
| **Media** | Personal and shared photos, music, videos, ebooks and audiobooks; libraries, albums, playlists, bookshelves, viewers/readers/players and personal reading/listening progress | ResourceFacade storage and media-access APIs, drive sharing, versions, image processing, search and streaming; new catalog and dedicated screens. Support git on Postgres, git on filesystem, DbResource and plain local filesystem. |
| **Comms** | Family and friend conversations, email, chat, calls and video | Most Comms functionality and its framework mail/chat/call services. Household rooms, selected contacts and family-oriented entry points replace business directory assumptions. |
| **Projects** | Home improvements, trips, hobbies, events and personal projects | Projects and shared work items, boards, milestones, calendar views, checklists, documents and discussions. Optional code and time tools remain useful for homelab and learning projects. |
| **School** | Schoolwork at home; homeschool subjects/curricula, learning plans, assignments, resources, portfolios and basic requirement records | Projects' shared work items and planning tools; Commons wiki, files, calendars and sharing. New curriculum/learner context organizes established external curricula and courseware, starting with direct resource links. |
| **Home & Auto** | Places and belongings, cleaning and chores, repairs, preventive care, vehicles, service history and manuals | Maintenance's facilities/assets/work model and planned procedures/schedules; Projects tasks; Commons calendars/files; selected inventory and expense functions. |
| **Kitchen** | Recipes, meal plans, shopping lists, pantry/fridge/freezer and leftovers | Commons sharing/calendar/files; Products' item/unit concepts; Warehouse's stock/location primitives where useful; selected Purchasing and Accounting concepts for shopping and receipts. The meal workflow is new. |
| **Money** | Account registers, income and spending, categories, bills/reminders, budgets, savings goals and reconciliation | Accounting's banking, payments, imports, reconciliation, budgets and ledger integrity, with a new simple personal-finance surface. |
| **Home Lab** | Personal-cloud setup and shortcuts, device sync, service readiness, storage/backup visibility and optional technical projects | Commons/Comms/Media as the everyday cloud apps, BOSS Admin's planned configuration surfaces, existing framework tools/monitoring, and Projects' git capabilities. |
| **Household** | Members and invitations, sharing, access choices, household preferences and app setup | Party/organization relationships and framework accounts; BOSS Admin's identity/access designs and shared settings patterns. No employment or payroll setup. |

Home Lab and Household will normally be less prominent than the daily apps. A regular member may only need
personal sync settings; a household organizer gets member/access management; a server operator gets instance tools.
These are separate responsibilities even when one adult performs all three.

Reports start inside their owning apps, using shared reporting tools and relevant Insights capabilities as they
become available. Sales, Marketing, Commerce, HR and the other business departments remain available through BOSS
for a home business; they do not each need a HOME counterpart.

### 3.1 Commons and Comms: the household's shared digital space

Start with personal and shared calendars, contact books, file spaces and notes. Useful household collections include
family documents, school files, appliance manuals, recipes, trip plans and a shared address book. Personal notes and
files remain private unless shared. A household calendar does not imply access to every member's private calendar.

Keep the existing capabilities for attachments, version history, collaborative notes, links to tasks and record
conversations. Expose a useful subset first and retain advanced tools for users who want them. **Media owns the
dedicated photo, music, video, ebook and audiobook experience**, including albums, bookshelves and the
viewer/reader/player screens. Commons keeps general file/drive management and can link a media file into Media;
it does not grow a parallel media library or book reader.

Comms supports household rooms, project conversations and selected family/friend groups. Email can use an existing
provider; operating a household mail server is optional. External messaging bridges and telephony are capabilities
to enable where configured, not prerequisites for family chat. Adults establish understandable boundaries for a
child's channels and contacts; within them the child can communicate independently, mute a conversation, ask for
help and request a new contact. Account administration does not automatically grant access to private messages.
Shared access and contact restrictions must be enforced by services as well as menus.

### 3.2 Media: photos, music, videos, ebooks and audiobooks

**Media is a standalone HOME app**, with its own launcher entry, access grant and dedicated screens for **Photos**,
**Music**, **Videos**, **Books & audiobooks**, **Albums & playlists**, and **Libraries & sources**. It combines
library management with a photo viewer, ebook reader and music/video/audiobook players. A Media user does not need
the Commons app-root grant, though both apps use the same ResourceFacade and access services.

**Personal and shared libraries.** A person can maintain private media and choose what to share with the household,
selected family/friends or another authorized group. A household can maintain shared libraries with separate view,
contribute and manage capabilities. Shared albums and playlists refer to media items; they do not make another
copy of each file. An item can appear in several collections while retaining one identity and source reference.
The visible library can span several explicitly selected storage sources, each with a chosen destination for new
uploads. Ownership, audience and physical storage are separate choices.

The initial media experiences are:

| Area | Library management | Viewer/reader/player |
| --- | --- | --- |
| **Photos** | Thumbnail grid and date/timeline browsing; albums, titles/captions, tags, favorites and search; capture date, orientation and available camera metadata | Dedicated full-size photo view, previous/next, zoom, fullscreen and slideshow. Preserve the original; any later edits or exports are explicit variants. |
| **Music** | Artists, albums, tracks, genres and available embedded tags/artwork; editable catalog metadata and personal/shared playlists | Audio player with a queue, play/pause, seek, volume, previous/next, repeat and shuffle; keep the queue while navigating HOME. |
| **Videos** | Titles, descriptions, collections, posters/thumbnails and available duration/format metadata; family recordings and other user-managed videos | Video player with seeking, fullscreen, volume, playback speed, supported subtitle/caption tracks and optional personal resume position. |
| **Ebooks** | Titles, authors, series, edition/language, covers, available identifiers and personal/shared bookshelves; associate alternate formats with the same book | Reader for an initial documented format set, starting with EPUB and PDF; contents/chapter navigation, search, readable layout/font controls where supported, bookmarks and personal reading position. |
| **Audiobooks** | Titles, authors, narrators, series, editions and covers; one book can comprise one file or an ordered set of parts, with chapter metadata where available | Book-oriented player with ordered part/chapter navigation, seek/skip, speed, sleep timer, bookmarks and personal listening position; retain the book queue while navigating HOME. |

**Personal progress, including shared books.** Store reading/listening state separately from the shared book and
its files, keyed by the authenticated user and a stable book edition/rendition identity. Each person has their own
not-started/in-progress/finished state, last position, bookmarks and optional private notes. Reading a shared book
requires view access to it, not permission to edit the library or original. Saving progress never writes into the
ebook/audio file or changes another person's position, including when the source is read-only.

Use a stable document location for reflowable ebooks, a page/location for fixed-layout documents, and a part or
chapter plus time offset for audiobooks. Percentage is a derived convenience, not the sole resume locator. Preserve
progress across authorized devices, renames and storage moves that retain identity. Use revision/conflict handling
so an older device update does not silently overwrite a newer position; deliberate rereading, resetting or seeking
backwards remains possible. If the content/edition changes, retain the old locator and reconcile or ask where to
resume instead of silently applying it to unrelated content.

Related ebook and audiobook editions can be linked in the catalog, but their positions remain separate unless a
reliable cross-format mapping is introduced later. Library owners and household organizers do not inherit access
to a person's progress or notes. A learner can explicitly share a reading result or selected notes with School;
shared-library membership never creates automatic reading reports. Removing book access blocks reading/playback
without making retained personal progress a route back to the content. Users can clear their own progress/history.

Use phone-friendly controls and accessible keyboard/label/caption support, with terminal alternatives for library
browsing, metadata and resource links. Start with direct playback of supported formats/codecs and explain when a
file cannot be played on the current client. A file extension such as `.mp4` is not a promise that every encoding
inside it can play. Preserve download/open-original access where authorized. Compatibility conversion, adaptive
streaming and hardware-assisted transcoding can follow; importing a library must not require converting its files.

**ResourceFacade is the storage boundary.** Media ingestion, catalog scanning, metadata extraction, previews,
reading and playback access resources through `ResourceFacade` and its `ResourceReference`/provider contracts.
Media must not implement separate filesystem, SQL/blob or git readers. Reuse existing facade operations and extend
their API for the media-specific access needs below; keep those extensions available to other apps and components.
Catalog records, collections, personal progress, access rules and source references live in the application
database independently of where the original bytes are stored.

**All four storage backends are initial scope**, with the same viewer/reader/player behavior and access rules:

| Storage choice | Behavior and reuse |
| --- | --- |
| **Git on Postgres** | A `git://` resource backed by a database git repository, with trees/refs/history in Postgres and large media optionally stored through git LFS in Postgres content blobs. Configure relevant LFS patterns explicitly; facade reads resolve the actual content and revision. |
| **Git on filesystem** | A `git://` resource backed by a local git repository, with git objects/history on disk and content served through the same facade API. Support binary content and transparent LFS resolution when enabled. Show the actual LFS payload location separately from the repository location; a fully disk-backed git/LFS choice needs filesystem LFS payload support in the framework. |
| **DbResource** | `dbresource://` files with their existing file history and content-blob handling for large objects. Store media in Postgres without requiring git or LFS; use the facade's streaming/version behavior. |
| **Plain local filesystem** | An authorized `file://` directory, including an existing mounted filesystem location, indexed in place. Large MP4s, books, music and photos remain ordinary files; no git repository or copy into Postgres is required. Support read-only and explicitly writable sources. A missing mount marks the source unavailable, without deleting catalog records or originals. |

A household might keep family photos in Postgres git/LFS and its video collection in a disk directory. Media should
present the same library and playback concepts across all four backends. Source settings show actual storage and
write policy; registering a disk root is a trusted operator action, after which authorized users choose among available
sources without receiving arbitrary filesystem access. Git on filesystem and plain local filesystem remain distinct
choices, as do database-backed git and DbResource.

Existing [drive services](../../../../docs/data/Revision-Management.md#drives) accept privileged root locations,
while their ordinary folder option defaults to database resources. Existing
[git/LFS services](../../../../docs/interfaces/Git-Hosting.md#large-files-git-lfs) store LFS payloads as content blobs.
That currently places LFS payloads in Postgres even for a filesystem git repository; do not label that arrangement
as entirely on disk. Correct this through the framework's LFS/provider storage, defaulting payload placement to the
repository backend and making any override explicit; track the fix in
[Remaining work: Git and git hosting](../../../../plans/Remaining-Work.md#git-and-git-hosting).
No pre-release data migration is required; the data model becomes stable at the first release.
The existing primitives do not yet supply the complete Media workflow or the expanded facade contract. Design
source registration and indexing explicitly, including root confinement, symlink handling and source access on
each serving node.

**ResourceFacade API extensions.** Detailed method names remain to be designed; the required contract is:

| Need | Shared facade/provider behavior |
| --- | --- |
| Resource description and capabilities | Length, content type, stable identity/revision or change validator, availability, and supported read/seek/write/version operations without reading the entire object. LFS size/type describe the resolved content. |
| Bounded streaming and seeking | Open a stream or byte window at an offset, with independent readers, cancellation and bounded memory. Resolve git/LFS, DbResource blobs and filesystem files behind one API. Extend providers for efficient partial access; any necessary spooling/caching belongs behind the facade and is bounded and reusable, not repeated whole-file loads in Media. |
| Consistent reads and delivery | Pin or validate the content revision across reader requests and seeks; detect changed content rather than mixing versions. HTTP adapters translate these operations into HEAD/GET, ranges and conditional responses with appropriate lengths and validators. Authorization remains mandatory before serving each request. |
| Compound books and related resources | Resolve authorized ebook package members, images/styles, audiobook parts, chapter data, covers and subtitle files through the same resource contract. Constrain resolution to the approved item/package; resource names cannot escape into unrelated files. Ebook presentation must isolate active content and external requests. |
| Processing and derived content | Metadata probes, thumbnails, covers and optional conversions consume facade streams or controlled seekable handles. If a decoder requires a local file, the framework manages a scoped temporary handle. Derived content is keyed to source revision and has its own storage/cleanup policy; no app-specific bypass to raw database blobs or disk paths. |

ResourceFacade abstracts byte access and provider capabilities; media-format interpretation belongs in reusable
framework media services, while catalog and personal-progress rules remain domain services. Neither a resource
URI nor a capability flag grants access. Shared Media/drive authorization resolves the permitted resource and
revision before invoking the facade, and related resources inherit that checked scope.

**Library ingestion and maintenance.** Offer upload/import into a chosen managed source and “Add existing folder”
for indexing in place. Scan through the facade for supported media, extract available metadata, generate
thumbnails/posters/book covers and allow manual corrections without rewriting originals by default. Keep a stable
media ID, source/path and content revision
or fingerprint so a rename, rescan or changed file can be reconciled and derived previews refreshed. Flag possible
duplicates for review; do not silently merge ownership or delete duplicate files. Catalog changes and file changes
need a shared reconciliation path whether made through Media, Files, git or directly on disk.

Scanning and metadata/preview jobs run in bounded batches, can be retried and report failures by file while
continuing with independent items. A temporarily missing file is marked missing; permanent removal is a separate
decision. Distinguish removing an item from an album, removing it from the library, trashing a writable original
and permanently deleting content. Git history and LFS retention still apply; removing a visible item is not a
promise that its historical bytes were erased.

**Large files and reading/playback delivery.** Reuse and extend the framework's streamed responses and byte-range
support over ResourceFacade; read only the needed portions instead of loading a whole movie or book into application
memory. Serve resolved content on every backend, never an LFS pointer. Check each request's access, content type,
range and revision; thumbnails, covers, ebook members, audiobook parts, subtitle files and variants follow the same
scope.
Confirm the current upload/LFS size limits against the intended media sizes, and design configurable limits and
resumable large uploads where needed. Indexing existing disk media must not depend on an upload-size allowance.

Keep originals separate from reproducible thumbnails, extracted metadata and optional playback variants. Cache
derived content by source revision and processing settings, with quotas and cleanup; never expose cache paths as
an authorization shortcut. Use the same reader/player/viewer primitives from record attachments or School resource
links when useful, while Media owns its library-oriented screens.

**Sharing and privacy.** Media and the underlying resources must have one effective access policy. A share grants
only the selected item/collection and permitted actions, not access to an entire drive or git repository. Repository
clone access includes history and must not be granted merely to view an album. Equally, Files, DAV, git or a direct
stream URL must not bypass a restriction that Media claims to enforce. Recheck grants for playback/downloads and
new range requests after revocation. Explain that a recipient's already downloaded copy cannot be recalled.

Titles, search results, thumbnails and embedded metadata can be sensitive too. Make location/camera metadata
disclosure a deliberate sharing/export choice while retaining the original. Reading/listening/viewing history and
resume state are personal, optional convenience data, not household monitoring reports. Children can organize their own
media and enjoy selected shared libraries within their configured access; access administration does not expose
private libraries or generate activity reports. Face recognition and AI media analysis are not initial requirements,
and an album share does not automatically authorize sending its content to an AI provider.

**Ownership and follow-up work.** Reuse ResourceFacade, drive/share rules, git/LFS, DbResource, streaming, image
tools, jobs and search. The facade/provider extensions, media-format services and viewer/reader/player widgets
belong in the framework; shared library/catalog and personal-progress records/services follow Mantle's ownership
rules; HOME supplies the Media app and household workflows. Backups must cover the catalog, personal progress,
original bytes and versions in each configured source, including the actual LFS payload store. Disk sources require
their own backup; database-backed git, DbResource and Postgres content blobs are covered by the database backup.
Changing storage later should use facade copy/verify/relink operations preserving media/book IDs, collections and
personal progress, with source deletion a separate choice.

Later work can add automatic phone/camera uploads, richer photo organization/editing, metadata-provider
integrations, casting, offline media and transcoding. The initial app scope includes photos, music, videos, ebooks
and audiobooks, their viewers/readers/players, personal progress even in shared libraries, personal/shared access
and all four required storage backends through ResourceFacade.

### 3.3 Projects and personal tasks

Keep the strengths of Projects: work breakdown, assignments, due dates, milestones, boards, timelines, documents
and discussion. A kitchen renovation can be a substantial project; a birthday party can be a short checklist.
Billing, client rates and utilization are absent from ordinary HOME navigation. Time tracking, repositories and
more detailed planning are optional capabilities rather than a separate reduced project engine.

Personal and family projects remain useful alongside School. A learner's science fair project, coding hobby or
field trip can link to a School learning plan without duplicating its tasks, files or discussion. Projects keeps
the general project tools; School owns the curriculum and learner context.

Chores, School work and project tasks appear together in Today, with one identity and one owner workflow per task.
Home & Auto owns chore schedules and completion rules; School owns learning plans and any explicitly required
assignment review. Appearing in another app's view never creates a second completion record.

### 3.4 School: schoolwork at home and basic homeschooling

**School is a standalone HOME app**, with its own launcher entry, access grant, landing page and settings. It
supports both work brought home from a school and learning organized at home using established curricula, books
and external courseware. A household can use either or both. School reuses shared task, calendar, wiki and file
tools without requiring the learner to have the Projects or Commons app-root grant.

The initial sections are **My learning**, **Subjects & curricula**, **Planner**, **Resources**, and **Portfolio &
records**. Learners primarily see their own plans, resources and work; adults organizing homeschooling can maintain
the relevant curricula and household learning plans. A homework-only user can enter assignments immediately without
setting up a homeschool curriculum.

**Schoolwork and learner autonomy.** Start with subjects or courses, assignments, due dates, reading/practice
routines, supporting files and learner-managed progress. A learner can add their own work and interests, plan a
week, break an assignment into steps and choose reminders. The default flow is “To do”, “Working on it” and “Done”.
“I'd like help” or “Ready to review” invites support; required review is an explicit setting for a particular
assignment or requirement. Parents can collaborate on planning when helpful. Curriculum pacing should allow the
learner and supporting adult to adapt the plan rather than treating every missed planned date as a failure.

**Basic curriculum planning and management is initial scope.** Organize an academic year, term or flexible date
range around learners, subjects, goals and the established curricula the household has chosen. Keep curriculum
title, provider/author, edition or version, source link and relevant books/materials together. A curriculum plan
can reference an external course or a sequence of textbook chapters just as easily as individual online resources.

The planner should support:

- A reusable subject outline with units/topics, learning goals, suggested order, materials and optional target
  dates. Adapt an external curriculum's outline for the household without building its instructional courseware.
- Assigning an outline to one or more learners, with each learner's pace, work and completion kept separately.
  Shared activities can have several participants without marking every learner complete when one finishes.
- Planning a week from the outline: readings, external lessons, practice, discussions, experiments and outings,
  alongside ordinary school homework. Tasks and calendar events link to the relevant unit and resource.
- Moving, skipping, replacing or revisiting planned work with a short note when useful. Preserve completed work's
  curriculum/version reference when revising a plan or reusing it for a later term or another learner.
- A simple view of planned, in-progress and completed coverage by learner and subject, with optional feedback or
  a recorded result from an external course. Completion and demonstrated understanding are separate observations;
  neither requires an automatic grading engine.

**Wiki and resource library.** Use Commons wiki spaces/pages for subject guides, unit outlines, reading lists,
planning notes and learner notes, with the existing revisions, co-editing and sharing. Link those pages to the
structured plan, assignments and files. The structured records own scheduling, participation and completion;
wiki pages provide the readable material around them. Private learner notes stay private unless shared, and a
subject guide's visibility does not expose every learner's work.

Begin online-content support with direct links: for example, a selected Wikipedia article, an educational video,
a museum resource, a publisher's lesson or a course page on a learning platform. A resource entry needs a title,
URL or file/book reference, source, related subject/unit and optional reading instructions. A wiki page can collect
these into a useful study guide. Open the original resource with its destination clear; the initial feature needs
neither an embedded viewer nor automated copying of external content. Textbooks and offline activities remain
equally usable sources.

Adults can curate shared learning resources, and learners can suggest additions. Link selection and Child Assist
source access are distinct: adding an article to School does not authorize AI to browse its entire site or gain
access to an external course account. Apply the child's configured contact/content boundaries and explain when a
link leaves HOME. A direct link does not make the external site's content or controls part of HOME's enforcement.

**Basic homeschooling records.** Let the household define the requirements it needs to track for each learner and
period: a subject or goal, source/reference, target or due date, related planned work and evidence, and a completion
or review state. Support a short checklist and portfolio of selected work, reading/activity records and external
results. Where needed, include manually entered learning days or session/hours summaries and simple printable or
exportable records. Requirements and their source references are maintained by the responsible adult; HOME tracks
the chosen requirements and does not infer compliance from completed tasks.

These records support purposeful homeschool recordkeeping, not passive monitoring. No background measurement of
screen time, attention or behavior, automatic study-time reports, or harvesting of private notes and AI chats.
The learner sees which records are shared and can contribute their own work and reflections. Recordkeeping should
remain light enough that it does not displace learning; attendance/time fields are optional when not needed.

**Reuse and new work.** Reuse Projects' shared work items, lists/boards and planning tools; Commons' calendar,
wiki, attachments and sharing; and existing reporting/export mechanisms. Add a small domain model for curriculum
references/outlines, learner plans, resource associations and requirement/evidence records where needed. Subjects
and external courses do not each have to become a heavyweight project. Shared domain services belong in Mantle;
School owns its app context and educational vocabulary. Keep one wiki, file store and task system.

**Later integrations and scope boundary.** Over time, evaluate adapters for the major learning platforms actually
used by households: resource/course discovery, direct lesson launch, assignment import and selected progress or
result synchronization where supported. Choose each integration after checking the provider's interfaces, access
and data terms. Keep provider IDs and source/version information, reconcile imported work without duplicating it,
and make shared data explicit. Direct links and manual records remain usable if a connector is absent or unavailable.

Authoring actual courseware, delivering interactive courses, assessment engines and broader education management
are a separate project. Institutional student information, admissions, timetabling, accreditation and formal grading
systems are outside this HOME plan. A future education-focused **Ensuite** variant of mr-boss could address that
scope and share appropriate foundations. School supplies the basic planning, organization and recordkeeping needed
to use established curricula and, where applicable, external courseware; it does not depend on that future suite.

### 3.5 Home & Auto: care, chores and repairs

Organize around actual places and things: a home and its rooms, yard, appliances, tools, bicycles and vehicles.
Keep purchase/warranty details, manuals, service contacts, photographs, maintenance history and related receipts.
One household can have several homes or other locations; ownership and permission are separate from location.

Two closely related work patterns share the maintenance foundation:

- **Routine care and chores:** reusable instructions, a schedule, agreed responsibilities or optional rotation,
  reminders and self-reported completion. Members can volunteer, arrange their work and request a swap or help.
  “Clean the bathroom” should take very little setup. Completion records who did it and when for shared
  coordination; the next occurrence follows the chosen calendar or completion-based rule. Routine chores need no
  adult sign-off, proof photo, timer or automatic parent notification.
- **Repairs and preventive maintenance:** report a problem, choose a person or contractor, plan the work, attach
  instructions/parts, record completion and retain history. Examples include replacing a furnace filter, fixing a
  leaking tap and servicing a car. Vehicle care supports date and odometer/usage intervals when the shared due
  engine is implemented; the initial version can start with dates and manual readings.

Use simple task completion for ordinary chores. More involved work can use Maintenance's planned procedure and
checklist lifecycle, including required steps or adult acceptance for a stated reason such as work involving
hazardous tools. Show that requirement before the person starts; do not apply it to all chores because the assignee
is a child. A generic task button or a phone's CalDAV update must not bypass an explicitly configured requirement.
Recurring generation must avoid duplicate work and distinguish a completed, skipped, rescheduled or cancelled occurrence.

Parts and supplies are optional tracking. A receipt can link to Money without requiring a purchase order; a major
renovation can link to Projects without creating a second set of repair records. An allowance or reward, if wanted,
is an explicit household choice and Money action, not an automatic payroll consequence of finishing a chore.

### 3.6 Kitchen: meals first, inventory when useful

The core flow is **save a recipe → plan meals → build a shared shopping list → shop → cook**. It must work without
counting everything in the pantry. Recipes have ingredients, quantities/units, servings, steps, notes and source
links; manual entry is sufficient for the first slice. URL/import helpers can follow with a review step.

Meal plans allow recipes, leftovers, eating out and free-text meals. Generate missing ingredients into a list,
combine compatible quantities, keep incompatible units separate and let people add unrelated household supplies.
Show who is shopping and what is already checked off, with useful phone controls. Planning a meal does not itself
consume pantry stock or spend money.

Children with Kitchen access can suggest meals, add shopping needs and follow suitable recipes independently.
Instructions can identify steps needing adult help; that choice does not require adult approval of every meal
suggestion or list edit. The child Assist profile can help explain a recipe within its curated safety boundaries.

Optional inventory adds cupboard/fridge/freezer locations, approximate or precise quantities, minimum levels,
best-before/expiry labels, opened dates and leftovers. Cooking can suggest consumption for review; adjustments and
waste remain easy to record. Household dietary preferences and ingredient notes help planning without becoming a
nutrition or medical application.

Reuse item identifiers, units, stock operations and attachments selectively. A shopping-list line may simply be
“apples”; a recipe need not be a manufacturing BOM, and checking off groceries need not generate an ERP order or
stock-valued production run. Identify genuinely reusable recipe/ingredient, meal-plan and shopping-list records in
the detailed design rather than forcing them into unsuitable entities.

Grocy demonstrates the useful connection between stock, recipes, shopping and chores; Mealie demonstrates a
recipe/meal/list workflow and separate household meal plans. These are interaction references, not proposed runtime
dependencies. [Grocy features](https://grocy.info/), [Mealie documentation](https://mealie.io/).

### 3.7 Money: simple entries over reliable financial records

Present personal finance as a **single-entry, cash-based experience**: choose an account, enter money in or out,
the date, person/store, category, amount and optional receipt. Support transfers, split categories, recurring
reminders, monthly budgets, savings goals, account reconciliation and income/spending summaries. A planned bill
is a reminder/forecast until a payment is recorded; no invoice or receivable workflow is required for everyday use.

Proposed implementation: retain Mantle's balanced accounting underneath and generate the necessary postings from
the simple entry. Household categories map to configured accounts behind the scenes. Setup supplies appropriate
defaults; users do not enter debit/credit pairs, maintain posting rules or perform a business period close to track
groceries. This is a new adapter/workflow over Accounting, not a parallel ledger with different balances.

The detailed Money design must settle these semantics before implementation:

- Cash/bank income and expenses use the actual transaction date; expected income and bills are clearly separate.
- Transfers between the household's own accounts change balances without becoming income or spending. Opening
  balances and reconciliation corrections are identified separately from ordinary transactions.
- Card purchases appear once in spending; repayments transfer money to reduce the card balance. Cash-flow reports
  distinguish bank movements from card spending so neither purchases nor repayments are counted twice. Start
  delivery with cash/bank accounts, then add cards with these semantics explicitly tested.
- Imports stage and preview entries, detect repeats, match entries already recorded and retain the source. Failed
  independent rows are reported without discarding valid rows; one financial transaction remains atomic.
- Posted corrections use the existing reversal/adjustment rules. Changing a household label does not silently
  rewrite reconciled financial history. Reports explain whether they show spending, cash flow or planned amounts.

Money access is explicit and can be limited to particular personal or shared accounts. An adult role does not
automatically reveal every other adult's finances. A child might see a pocket-money account or savings goal without
seeing household balances. Household budget categories, purchase history and receipts follow the same scope.
Within that account's permissions, the child can maintain a budget, record spending and choose savings goals;
recording an entry does not authorize an actual payment. Any limits on moving real money are separate and explicit.

HOME does not initially add tax preparation, investment trading, business payroll or full receivables/payables.
Those who need business accounting use BOSS for the business organization. Live bank connections and expanded debt
or investment tracking can be evaluated after manual entry and statement imports are useful.

### 3.8 Household: membership and everyday administration

Create the household, add people, create or associate their accounts, choose app access, and establish shared
calendars, folders and rooms. A person can exist as a contact without a login; an invitation can attach to an
existing account without duplicating the person. Children need not have independent email accounts to participate.

Offer understandable capability bundles as starting points, with per-person adjustments: household organizer,
member, child/learner and invited participant. Keep money management, access administration and server operation
separate. “Child” is a configurable account experience and starting bundle. Adults choose its capabilities based on
the child's needs and ability, with autonomy within those boundaries. The settings explain to the child and adults
what each can see and change, which actions need review and why. Children can request help or a capability change;
ordinary account administration does not grant monitoring access to their private content.

Provide a dedicated **Child Assist** settings area for selecting curated activity profiles, approved AI services
and sources, allowed tools and data, and understandable usage/cost limits (§6.1). A child can inspect their effective
options and choose among enabled activities without changing the policy. Enabling an activity does not enroll the
child in conversation reporting. Changes to access, sharing and Assist options are visible to the affected user.

Initial settings include household name, locale, time zone, currency and units, available apps, notification
preferences and sharing choices. Reuse the shared account lifecycle and settings owners rather than creating a
second user database. Household managers may grant only capabilities within their delegated scope, not framework
administrator access. Invitations, delegated grants and account recovery need to follow the shared Admin work as
it becomes available; do not assume the draft invitation services already exist.

## 4. Accounts, organizations and sharing

### 4.1 Two independent organizations, one person's account

Use a Mantle organization party with the internal-organization role for **Jones Household**, and another for
**David E Jones Consulting**. Members have person parties and individual user accounts. Existing
[organization lookup](../../mr-mantle/service/mantle/party/PartyServices.xml) recognizes `PrtMember` as well as agent
and employee relationships to an `OrgInternal` party, so household membership need not masquerade as employment.

Do not make the business a child organization of the household merely because David owns it: current organization
lookup includes direct child organizations through `PrtOrgRollup`. Use independent memberships and explicit
relationships instead. David's HOME grants concern household activities; his BOSS grants concern consulting.

| Example context | Expected behavior |
| --- | --- |
| David in Jones Household | Household projects, vehicles, groceries and personal finances; HOME navigation. |
| David in David E Jones Consulting | Business customers, projects, invoices, accounts and reporting; BOSS navigation with the business organization selected. |
| Household member using only HOME | Only their authorized HOME apps and household/personal records; no BOSS launcher entries or access through direct BOSS URLs. |
| Relative invited to plan a reunion | The shared project, calendar, conversation and chosen files; no access to household finances or the general household directory. |

Every new record and mutation has an explicit, authorized owner/context. HOME defaults to a selected household,
and BOSS can retain its business context. The shared active-organization preference is a starting mechanism, not
enough by itself: changing context in another tab must not redirect an open form's write to a different organization.
Show the organization on forms where it matters, retain the record's owner, and validate it on submission.

Keep balances, budgets, purchases, assets and work distinct by owner. A business reimbursement or owner transfer
can link records across organizations only when the actor is authorized on both sides, with deliberate entries on
each side. Do not reassign a household receipt to the business or silently merge their reports. Shared contacts and
bookable resources can be linked explicitly; overlapping people never imply shared access to all their records.

### 4.2 App access is separate from record access

Use HOME app-root grants and permission bundles, without granting the corresponding BOSS roots merely to reuse a
screen or service. Domain services check the action, organization and record/share scope independently of the
mounting app. A HOME context can change labels and available features; it cannot confer authority.

**Known prerequisite:** [BossSetupData.xml](../../mr-boss/data/BossSetupData.xml) currently grants both
`BOSS_COMMONS` and `BOSS_COMMS` roots to `ALL_USERS`. Simply withholding other BOSS groups will therefore not satisfy
HOME-only access. Plan explicit opt-in BOSS root grants and HOME root grants, with a migration preserving intended
access for existing BOSS users. Update shared header/tray routes, record links, palette and quick-create providers
to resolve through authorized mounts. Avoid a blanket HOME-user deny that would also block someone using both suites.

Review broad existing service permissions and organization filters as part of that work. Global permissions alone
do not express “manage this household's money but not this business's books,” and some existing administrator paths
have wider scope than ordinary members. HOME organizer bundles must not inherit framework `ADMIN` or broad BOSS
finance-administrator authority as a shortcut. Add scoped domain checks where existing ones are insufficient.

Apply the same visibility rules to list rows, search, counts, notifications, attachments, exports, sync protocols
and Assist. Hiding a launcher icon or a column is not access control. Native app APIs and DAV clients must call the
same owner operations; explicitly required maintenance or schoolwork review cannot be bypassed by generic task
completion. These safeguards do not add approval steps to ordinary self-managed work.

### 4.3 Sharing beyond the core household

Default to explicit personal, household-shared or selected-participant visibility, chosen appropriately for each
record type. New household tasks may be shared; personal notes, mail and finance accounts start private. Household
membership alone does not override a private record's policy.

These defaults also apply to children. Shared responsibilities expose the status needed for coordination, while a
child chooses when to share a personal draft, question or project. Clearly identify the audience before sharing.
Any exceptional access to private content must use a separately authorized, documented path; being a household
organizer or enabling Child Assist is not that authority. Explain actual administrative/audit access and retention
without promising secrecy from the server operator.

Extended family and friends can become members when that is intended, or receive limited participation in a
project, album/folder, calendar, room or event without organization membership. A family relationship in Contacts
is descriptive and grants nothing by itself. Support ending participation and revoking shares without deleting
the underlying history. Another household can remain a separate organization and participate through selected shares.

Use authenticated accounts for ongoing participation. Existing secure links can serve specific sharing cases
when enabled; a general public family portal, social network or federation feature is not required initially.
Public publication, if later wanted, should be considered alongside PASS rather than exposing HOME roots anonymously.

## 5. Home Lab and the framework boundary

### 5.1 First scope: a personal cloud

“Home lab” initially means the self-hosted digital environment already established or planned in MR Moqui:
files and sharing, personal/shared media, calendars and contacts, notes and documents, mail, chat and calls, sync
clients, and optional git. The daily applications are Commons, Comms and Media; Home Lab provides a useful entry
point, setup/status views and operator tools for that same environment. It does not create another file store,
messaging service or calendar.

Nextcloud's home offering groups files, communication, groupware and office collaboration into a personal-cloud
experience. That is the reference scope, not a promise of Nextcloud app, client or API compatibility.
[Nextcloud for home users](https://nextcloud.com/home-users/), [Nextcloud Groupware](https://nextcloud.com/groupware/).

| Capability | Initial HOME treatment |
| --- | --- |
| Files, notes, calendars and contacts | Reuse the existing apps and shared records with household-oriented setup and sharing. |
| Photos, music, videos, ebooks and audiobooks | Media owns libraries and viewer/reader/player screens, with personal book progress even in shared libraries. ResourceFacade supports git on Postgres, git on filesystem, DbResource and plain local filesystem; show actual source/payload placement, readiness, usage and backup coverage. |
| Phone/desktop access | Explain supported DAV/SFTP clients and per-device app passwords, with address and revocation tools. Respect current limits such as shared CardDAV books being read-only and the documented CalDAV limitations. |
| Office documents | Retain the planned WOPI/external-editor direction from [BOSS initial design](../../mr-boss/plan/initial-design.md). Verify the available integration before promising in-browser office editing; files and native collaborative notes are independently useful. |
| Email, chat and calls | Use configured framework services; existing-provider email first is a valid household setup. Hosted mail, relay/gateway and call networking remain deployment choices. |
| Services and technical projects | Useful service links, setup information and authorized health; optional git repositories and documentation through Projects/Commons. No new hypervisor, container orchestrator or general remote administration product. |
| Storage, backups and recovery | Show actual supported status, failures and recovery instructions through shared operational tools. Account for the database (catalog/progress, git, DbResource and content blobs kept there), filesystem git and LFS payloads, plain media directories, configuration and required keys; a database backup alone is not every deployment's complete recovery plan. External editor/services have their own backup needs. |

Use the existing [deployment guidance](../../../../docs/Run-Deploy.md) and
[monitoring tools](../../../../docs/tools/Monitoring.md). Keep service installation, listeners and protocol
configuration in their framework/deployment owners. Household members need not see these details to use their apps.
True offline editing and Media's later background phone photo backup are separate capabilities, not implied by
WebDAV support.

### 5.2 Home automation is future research

The initial applications include **no Home Assistant-like automation or direct hardware control**. An asset
record, a manual meter reading and a maintenance reminder do not imply a controllable device.

A later research/design effort should compare integration with Home Assistant, direct device support, and a
combination. Home Assistant exposes authenticated REST and WebSocket APIs for interaction and live events, making
an integration a concrete option to investigate. This is an architectural inference, not a selected integration.
[Home Assistant REST API](https://contributors.home-assistant.io/core/internals/rest-api/),
[WebSocket API](https://contributors.home-assistant.io/core/internals/websocket-api/).

That study should inventory the existing Moqui Java PLC/device-control work and its MR Moqui port plans before
choosing new abstractions. It must decide together with the core framework work:

- Which reusable transport, connection, credential, event and command mechanisms belong in mr-moqui, and which
  device adapters belong in optional components rather than the core binary.
- How shared assets, device identities, readings and maintenance history relate across home and business uses.
- Which systems own automation rules and actual device state, and how availability, acknowledgments, retries,
  duplicate commands and authorization are handled when devices affect the physical world.
- What HOME presents to household users, what BOSS needs for industrial equipment, and where integration is more
  useful than reproducing Home Assistant's device ecosystem.

No initial HOME phase depends on resolving that work, and this draft makes no claim that the Java device
capabilities already have Rust parity.

## 6. Feel and day-to-day interaction

Follow the shared screen and accessibility conventions, with a HOME presentation profile rather than copied
widgets. Use the framework's theme and density mechanisms for a warmer palette and comfortable layouts; retain
contrast, dark mode, keyboard navigation, readable status words and terminal equivalents. The
[BOSS style guide](../../mr-boss/plan/style-guide.md) is the common foundation, with HOME deliberately emphasizing
phone use for shopping, chores, schoolwork and quick household coordination.

Prefer “Who's doing it?”, “Due”, “Done”, “Needs help” and “What's for dinner?” to staffing, work-order dispatch,
procurement or resource-utilization language on everyday screens. Keep short lists, recognizable icons with text,
clear next actions, forgiving edits and useful empty states. Adults can open richer planning views; capable older
children can use those views too, according to their capabilities and preferences.

Child-accessible views emphasize the child's own plans, creativity, shared responsibilities and communication.
Show enough instructions to act independently, with easy ways to ask for help, change a plan or say something
cannot be done. Optional encouragement can be light and pleasant; competitive leaderboards, penalties, streaks
and rewards are not default household policy. Children can choose reminder frequency and quiet hours within any
clearly stated household constraints. Finishing a small chore should take a few taps.

### 6.1 Assist: ordinary access and curated child options

**Non-child accounts** use substantially the same Assist capabilities, access controls, approval rules and limits
as business users, subject to their actual permissions and instance configuration. HOME does not impose the child
profile on other accounts. Assist is optional to every core workflow.

**Child accounts** get a curated variation of the same Assist system when enabled. The goal is independent access
to a useful tool for learning, making things and organizing life. Friendly language, suggested starting points and
free-form questions within an enabled activity should all be supported; an adult need not approve each question.
The following are proposed starting options, enabled individually by the responsible adult:

| Curated option | What the child can do | Initial boundary |
| --- | --- | --- |
| **Learn and create** | Ask for explanations, examples, practice, feedback, stories and ideas for personal projects | Reviewed learning/creative skills, suitable model configuration and content safeguards; selected permitted materials, with no general web or external-action tools. This is the recommended first option when enabling AI. |
| **Plan and organize** | Break down their own work, plan a week, draft a checklist or adjust a reminder | Only permitted personal/shared records and reviewed actions. The child previews and confirms a change under the normal Assist rules; a routine change does not require an additional adult decision. |
| **Explore approved sources** | Ask research questions against selected reference collections and follow source links | An explicitly enabled, curated retrieval option with source restrictions and content checks. No unrestricted browsing or arbitrary external connections in the initial child profiles. |

School's selected curriculum pages and resource collections can supply context for these activities when authorized.
The learner can ask for an explanation or help planning a unit; Assist does not automatically assess mastery, mark
curriculum requirements satisfied or send learning reports to adults. School remains fully usable without AI.

An adult may also leave AI disabled. Curated options are product requirements to design and validate, not a claim
that any particular model, provider or prompt is sufficient for child safety. Keep the child profile understandable:
show what it can help with, what information it uses, and how to stop or ask a person for help.

**Safety and enforcement.** Build the child profile around a reviewed combination of models/providers, skills,
sources, tools and data policy. Evaluate response suitability, provider data handling and safety behavior before
offering a combination; local hosting alone is not a safety guarantee. The profile should:

- Give helpful, age/ability-appropriate explanations, make uncertainty clear, and handle harmful or unsuitable
  requests safely. Encourage human help where needed, including for risky physical activities. Present AI as a
  tool, with no pressure to keep using it, keep secrets from trusted people or substitute it for human relationships.
- Enforce content safeguards on inputs, retrieved/attached material and outputs, including what is streamed to
  the child. Treat document/web instructions as untrusted data. A restricted source list alone does not make all
  content or generated answers suitable.
- Use only the child's authorized context, narrowed further to the activity's permitted data. Make attachments
  and sharing deliberate; keep secrets and unrelated household/business records out of model requests. Choose
  provider disclosure, content retention and optional memory settings explicitly, with minimal data by default.
- Enforce the profile server-side across every enabled AI entry point, including chat agents, text actions, model
  selection/fallback and tools. Changing agents, starting a new chat or using another client cannot select the
  ordinary adult profile. If an approved path or required safety control is unavailable, explain that and keep the
  activity unavailable rather than falling back to unrestricted AI.
- Keep model/tool/configuration administration outside the child bundle. Reviewed actions intersect the child's
  permissions with the activity policy; they never run with an adult's credentials. Initial profiles exclude
  payments, public publication, contact with new recipients and arbitrary code execution. Normal app access and
  explicit domain review requirements still apply, and the child cannot approve an action beyond their authority.

**Privacy and help.** Do not automatically mirror transcripts, infer behavior for adult dashboards or send routine
AI-use digests. Give the child an easy way to share a chosen question or result with a trusted person, showing the
recipient and content first. Content safety processing is distinct from granting adults access to conversations.
Any future automatic safeguarding notification needs a separately specified, narrow and transparent policy for
its trigger, recipient and minimum necessary information; general conversation monitoring is not an initial feature.

Reuse the [framework Assist](../../../../docs/ai/Assist.md) runtime, model selection, permissions, tool policies,
approvals and audit mechanisms. Its documented separation between administration and transcript access
(`AgentAudit`) matters here: household/Child Assist management must not automatically grant transcript-audit rights.
The child profiles, curated catalog, content safeguards and consistent enforcement across entry points are **new
design and implementation work**. Put reusable enforcement in the framework; HOME supplies the household choices
and child experience. Do not expose child AI access until those controls are implemented and verified.

## 7. Delivery proposal

This is an initial suite direction, delivered in usable slices. Before each slice, produce the relevant detailed
design and identify which shared dependencies are built, need extension or remain deferred.

| Stage | Deliverable | Completion example |
| --- | --- | --- |
| **H0: access and shared foundation** | HOME roots and context, Jones Household and consulting fixtures, scoped membership/capability model, explicit Commons/Comms root access, HOME-aware shared links; child autonomy/privacy defaults and Assist policy contract | A HOME-only user cannot discover or open BOSS apps; David can use both organizations without records or form writes crossing contexts; a child account has no route to unrestricted Assist. |
| **H1a: household personal cloud** | Today, essential Household setup, Commons/Comms mounts, basic sharing and Home Lab sync/setup information; the first curated Child Assist option when its controls are ready | Household members share a calendar, folder and conversation; a relative sees only shared items; a child's personal note and AI conversation are not automatically exposed to an organizer. |
| **H1b: Media** | Photo/music/video/ebook/audiobook libraries and viewers/readers/players; personal/shared collections and per-user book progress; ResourceFacade media APIs, source setup and indexing on all four backends | People read/listen to the same shared book with independent progress, browse photos and play music/video; equivalent facade-backed reading/seek behavior works on Postgres git, filesystem git, DbResource and plain local files. |
| **H2a: work around the home** | Projects plus Home & Auto's belongings, self-managed chores and repair tracking | A child plans and completes ordinary work independently and can ask for help; explicitly chosen review works; a repair retains its manual and history. |
| **H2b: School** | Standalone School app, homework organization, basic curriculum outlines and learner plans, wiki/resource links, weekly planning and requirement/portfolio records | A household plans a term using an external curriculum, links wiki guides and Wikipedia/resource pages, tracks each learner's work and exports selected homeschool records; a homework-only learner starts without curriculum setup. |
| **H3: kitchen** | Recipes, weekly meals and shared shopping first; optional pantry tracking next | A household plans meals, combines ingredient needs, shops from phones and records leftovers without configuring inventory accounting. |
| **H4: money** | Household cash/bank entry, categories, imports and reconciliation; then cards, budgets and cross-app receipt links | A grocery purchase is entered once, matched to a statement and linked to its receipt; an account transfer is excluded from spending; business records remain separate. |
| **H5: deepen the shared capabilities** | Richer preventive care, vehicle usage intervals, optional advanced projects, office integration, selected learning-platform adapters, Media camera uploads/transcoding and improved Home Lab operations as shared foundations mature | Household use drives improvements shared with BOSS, with a documented dependency and acceptance scenario for each addition. |

H1b builds on the shared access/resource foundation; its five media types, personal book progress, ResourceFacade
extensions and all four storage choices are initial scope, independent of later camera uploads or transcoding.
H2a and H2b can proceed independently once their shared
foundations are ready; H3 and H4 can change order after review. School's basic curriculum planner belongs in its
initial delivery, while platform integrations come later.
Complete maintenance procedures and preventive scheduling depend on the shared Maintenance work; H2a should remain
useful with basic tasks and repair history. Home Assistant/device work has its own later research track, outside
these stages.

The non-AI household apps can ship before Child Assist. Child AI availability is gated on the curated profile and
its safeguards, without delaying ordinary Assist for authorized non-child users.

Use fictitious people and records around the two requested organization names in demo/spec fixtures; do not infer
real family members, balances or credentials. Maintain focused acceptance cases for HOME-only, dual-suite,
child/learner, limited guest and household organizer users. Exercise unauthorized direct requests and protocol
access as well as visible navigation, and check the main flows at phone width and in `tui`.

Include autonomy and child-safety acceptance scenarios: a child creates a personal project, adjusts reminders and
completes an ordinary chore without adult intervention; requests and selectively shares help; and sees a clear
reason when an activity needs adult review. Verify that organizer views do not expose private notes or transcripts.
Before enabling Child Assist, check safe handling of unsuitable content, untrusted source instructions, unauthorized
data/actions and attempted profile bypass through other AI surfaces or model fallback. Also verify useful allowed
learning and planning, so safety controls do not reduce the experience to blanket refusal. Non-child users retain
their normal business-style Assist behavior.

School acceptance cases include a learner with School access and no Projects/Commons app-root grants; one
curriculum reused by two learners with independent pace and completion; a linked project whose tasks appear once
in Today; and a household exporting only its selected requirement records and portfolio items. Changing a curriculum
outline must preserve completed work's context. Direct resource links must work without a platform connector or AI,
and an export must not pull in private learner notes or conversations.

Media acceptance cases cover the same facade contract on git/Postgres, git/filesystem, DbResource and plain local
filesystem, including LFS resolution and the configured payload placement. Cover personal/shared libraries, photo
browsing without a Commons grant, music queues/playlists, video playback/seek, ebook navigation and multi-part
audiobook playback with bounded memory. Verify that a subset share exposes neither neighboring files nor repository
history, and that revoked access also blocks new original/thumbnail/range requests. A corrupt file must not stop an independent
scan item; an unavailable directory must not delete the catalog. Test rescan/rename, read-only sources, collection
removal versus original deletion, backup coverage and clear unsupported-format states. Exercise ordinary cases
with small fixtures; run a specific large-file or long-playback scenario only with explicit permission if it is slow.

Two users reading/listening to a shared book must retain independent positions, bookmarks and completion states,
including on read-only sources. Check personal progress across devices, deliberate resets, stale updates, storage
moves and changed editions; library managers must not see it without a separate explicit share. Verify that ebook
package members cannot escape their scope, a seek cannot mix content revisions, and revocation blocks content
even when the user's progress record remains. Run these backend cases through ResourceFacade rather than testing
only storage-specific helpers.

Implementation follows the repository's fast-spec practices. Slow scenarios are only run when their specific
output is needed and the user explicitly authorizes them; they are not a routine coverage step. This document-only
draft requires no build or application test run.

## 8. Questions for the next design conversation

The following choices remain proposals rather than settled requirements:

1. **Remaining app boundaries and names:** School and Media are separate apps, as decided. Keep chores inside
   Home & Auto or give them a dedicated launcher entry over the same records? Is Household best presented as an
   app or a settings area?
2. **Home Lab navigation:** is a distinct personal-cloud/setup hub useful, or should its ordinary-member features
   live entirely in Commons settings with an operator-only Lab app?
3. **Personal finance depth:** confirm the hidden balanced-ledger approach, card/spending versus cash-flow
   semantics, account-level privacy, and the priority of imports, budgets and allowance tracking.
4. **Household detail:** how much pantry tracking, chore rotation and schoolwork organization should ship as optional
   defaults? Which specific activities benefit from an explicit review step while preserving self-completion for
   ordinary work? Which concrete family routines should supply the first end-to-end examples?
5. **Extended participation:** which first scenario matters most—shared trips/events, family files/photos,
   conversations or collaboration between separate households?
6. **Delivery priority:** confirm whether daily coordination and chores should precede Kitchen and Money, or move
   one of those apps earlier to make the suite useful for the first household.
7. **Child Assist curation:** which initial learning, creative and planning activities, reference collections and
   provider/model combinations should be evaluated first? Set concrete suitability, privacy, retention and usage
   criteria before release. Child autonomy and safety are established guidelines; the open choice is how best to
   deliver them, not whether to add routine child monitoring.
8. **School examples and integrations:** which established curricula, planning styles and homeschool record formats
   should guide the first implementation? Which learning platforms matter most after wiki pages and direct links?
   The standalone app and basic curriculum manager are initial scope; courseware and broader education management
   remain a separate future project.
9. **Media priorities:** which photo, music, video/subtitle, ebook and audiobook formats and devices should guide
   the first viewer/reader/player checks? Which collections need each backend and which need write access? All five
   media types, personal book progress and all four ResourceFacade backends are initial scope. Detailed API names,
   reader format coverage and LFS payload configuration remain to design; richer editing, phone backup and
   transcoding can be prioritized separately.

## 9. Research basis

Local design and implementation references are linked at the relevant decisions above. They establish the reusable
foundation, not an assertion that every proposed BOSS capability is complete. The current component sources were
checked for the dependency chain, organization relationships, root grants, placeholder status and media's existing
ResourceFacade/version, drive/root-location, DbResource, git/LFS content and streaming foundations.

External research was limited to primary project sources, checked November 1, 2026: Nextcloud for the personal-cloud
scope, Grocy and Mealie for household kitchen patterns, and Home Assistant's APIs for the future integration
question. These inform the proposal; none is a required dependency or a commitment to feature parity.
