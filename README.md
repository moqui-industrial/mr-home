# IronStone HOME - Home Organization-Mechanization Ensuite

> **In design, and not ready for production (before 0.1.0).** HOME is a design so far: this repository has the plan
> and no screens to run yet. [MR Moqui](https://github.com/moqui/mr-moqui#readme) and IronStone BOSS, which it will
> build on, are in early development: their data model and stored data change without migrations, and a security
> review is under way with its findings recorded openly in the mr-moqui repository. Run them only on development
> machines, with no real data or credentials, and not exposed to the internet. The full notice is at the top of the
> [mr-moqui README](https://github.com/moqui/mr-moqui#readme).

IronStone HOME is a planned suite of household applications for [MR Moqui](https://github.com/moqui/mr-moqui), the
Moqui framework in Rust. It helps people organize life at home: their shared digital space, photos, music, videos and
books, maps and places, conversations, projects, schoolwork and homeschooling, chores, the house and vehicles, the
kitchen, and money. It should be useful to one person, a family, or another household arrangement, with selected
participation by extended family and friends.

HOME uses the capabilities behind [IronStone BOSS](https://github.com/moqui/mr-boss), the business suite, with
applications designed around what a household does. A meal plan starts with what to eat this week; cleaning starts
with what needs doing; personal finance starts with money coming in and going out. Nobody should have to learn
purchasing, manufacturing, HR or bookkeeping to do these things.

## What sets it apart from BOSS

- **Household wording and feel.** Warmer and less formal, with "Who's doing it?" and "What's for dinner?" where BOSS
  has assignments and orders, and more weight on the phone.
- **Children act on their own.** A child can plan, learn, create, contribute and ask for help within what the adults
  of the household have chosen. HOME is not a tool for monitoring children, and child safety is a high priority,
  including a curated, server-enforced variation of Assist.
- **One installation, a household and a business.** A household and a business can be separate organizations on one
  server. One person uses both with one account; another household member sees only HOME, and the BOSS apps stay
  out of their sight and reach.
- **A personal cloud first.** Home Lab begins as the self-hosted files, media, calendars, contacts, mail, chat and
  calls MR Moqui already has, with setup and status in one place. Home automation is later research.

## The planned apps

**Today** is the landing page: a person's appointments, schoolwork and chores, the household meal plan and
reminders, each linking to the app that owns it.

| App | For |
|---|---|
| Commons | Calendar, contacts, files, notes, household knowledge and personal tasks |
| Comms | Family and friend conversations: email, chat, calls and video |
| Media | Personal and shared photos, music, videos, ebooks and audiobooks, with each person's own reading and listening progress |
| Maps | Places, personal and shared, with maps beside addresses, usable from local map data |
| Projects | Home improvements, trips, hobbies, events and personal projects |
| School | Schoolwork at home, and homeschooling: subjects and curricula, learning plans, assignments, portfolios and records |
| Home & Auto | Belongings, cleaning and chores, repairs, preventive care, vehicles, service history and manuals |
| Kitchen | Recipes, meal plans, shared shopping lists, and the pantry, fridge and freezer when that is useful |
| Money | Account registers, income and spending, bills and reminders, budgets, savings goals and reconciliation, as simple entries over a balanced ledger |
| Home Lab | Personal-cloud setup, device sync, service readiness, storage and backup visibility |
| Household | Members and invitations, sharing, access choices and household preferences |

Sales, Marketing, Commerce and the other business departments stay in BOSS, for a home business.

## State

The [initial design](plan/initial-design.md) is a draft for discussion: the application set, the shared foundations,
accounts and sharing, and a delivery order from access and the personal cloud through Media, Maps, work around the
home, School, Kitchen and Money. It ends with the [questions still open](plan/initial-design.md#8-questions-for-the-next-design-conversation).
Nothing is built: the component has its `component.xml`, which depends on `mr-boss`, and the plan.

To follow along, get it beside BOSS in an [mr-moqui](https://github.com/moqui/mr-moqui#quick-start) checkout:

```sh
./moq component-get mr-home   # With mr-boss and mr-mantle, which it depends on
```

## License and Contributing

[CC0 1.0 Universal with a Grant of Patent License](LICENSE.md). To contribute code, sign
[AUTHORS.md](AUTHORS.md) in a commit from the account that submits the pull request; its Contributing
section has the details.
