# Rails Design Principles - DHH Standards

## The Majestic Monolith
- **Prefer monoliths over microservices** - Start with a single, well-organized application
- **Embrace vertical integration** - Keep related functionality together in one codebase
- **Scale through simplicity** - Most applications don't need the complexity of distributed systems
- **Modularize within the monolith** - Use engines, concerns, and clear boundaries instead of separate services

## Convention over Configuration
- **Follow Rails conventions religiously** - Use standard naming patterns, file structures, and approaches
- **Minimize configuration files** - Let Rails defaults handle most decisions
- **Use Rails generators** - Leverage `rails generate` for consistent file creation
- **Stick to RESTful routes** - Use the standard 7 RESTful actions whenever possible
- **Follow MVC strictly** - Keep models, views, and controllers in their proper roles

## No Silver Bullets Philosophy
- **Avoid premature optimization** - Don't solve problems you don't have yet
- **Resist framework switching** - Stick with Rails rather than chasing new frameworks
- **Question complexity** - If it feels too complex, it probably is
- **Start simple, evolve gradually** - Begin with basic Rails patterns and add complexity only when needed

## Database-Centric Design
- **Active Record is your friend** - Embrace AR patterns and conventions
- **Use database constraints** - Leverage DB-level validations and constraints
- **Migrations are sacred** - Keep them clean, reversible, and well-documented
- **Avoid complex ORMs** - Don't fight Active Record, work with it
- **Normalize appropriately** - Follow good database design principles

## Hotwire First
- **Server-rendered by default** - Generate HTML on the server, enhance with Hotwire
- **Turbo for navigation** - Use Turbo Drive and Frames for fast page transitions
- **Stimulus for interactivity** - Add JavaScript behavior with Stimulus controllers
- **Avoid complex JavaScript frameworks** - Skip React/Vue unless absolutely necessary
- **Progressive enhancement** - Ensure functionality works without JavaScript

## Testing Philosophy
- **Test what matters** - Focus on integration tests and system tests
- **Avoid over-testing** - Don't test framework functionality
- **Use Rails testing conventions** - Leverage test/fixtures, not factories
- **Keep tests simple** - Tests should be readable and maintainable
- **Test behavior, not implementation** - Focus on what the user experiences

## Code Organization
- **Fat models, skinny controllers** - Business logic belongs in models
- **Use concerns judiciously** - Share code through well-named concerns
- **Service objects** - NEVER use them, prefer concerns and PORO
- **Keep views simple** - Logic belongs in helpers or models, not templates
- **Organize with engines** - Use Rails engines for major feature boundaries

## Performance Principles
- **Measure first, optimize second** - Don't guess at performance problems
- **Caching is king** - Use Rails caching at multiple levels
- **Database queries matter** - Understand N+1 problems and use includes
- **CDN for assets** - Serve static assets from CDN
- **Background jobs for heavy work** - Use ActiveJob for time-consuming tasks

## Security by Default
- **Use Rails security features** - CSRF protection, SQL injection prevention
- **Strong parameters** - Always use strong parameters in controllers
- **Authentication with care** - Use proven gems like Devise
- **Authorization patterns** - Implement consistent authorization checks
- **Validate all inputs** - Both client-side and server-side validation

## Deployment and Operations
- **12-factor app principles** - Environment-based configuration
- **Heroku-style deployment** - Simple, push-based deployments
- **Environment parity** - Keep development, staging, and production similar
- **Log everything important** - Use Rails logger effectively
- **Monitor key metrics** - Track performance and errors

## Team and Process
- **Code review everything** - No code goes to production without review
- **Commit messages matter** - Write clear, descriptive commit messages
- **Refactor continuously** - Keep code clean through regular refactoring
- **Document decisions** - Use README files and inline comments
- **Share knowledge** - Pair programming and knowledge sharing sessions

## Anti-Patterns to Avoid
- **Don't abstract too early** - Avoid creating abstractions before you need them
- **No callback hell** - Keep Active Record callbacks simple and minimal
- **Avoid deep nesting** - Keep controller and view logic flat
- **Don't bypass Rails** - Work with the framework, not against it
- **No premature microservices** - Start monolithic, extract services only when necessary