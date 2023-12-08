# lmp1777-recreate
Recreate for https://github.com/OpenLiberty/ci.maven/issues/1777

# GOOD
`mvn liberty:run`

Hit browser
 * http://localhost:9080/converter
 * http://localhost:9080/converter2
 * http://localhost:9080/converter3

# BAD (PROBABLY)

Do `mvn liberty:dev` and observe error in log

We can probably stop there, though you'll notice with the app here it's likely going to seem to work just fine.   That's because the initial `compiler:compile` mojo 
compile will have been done when dev mode starts initially.   So what's missing is the ability to make an update.

There's one other complication... the ordering is a bit unpredictable, because we go from a Set to an Iterator.   So we have to get lucky/unlucky in order to see the problem.
It's possible for the ordering for a given sub module to end up correct, if the Iterator derived from the Set happens to order the classpath entries collection, in which case a compiler error
wouldn't be seen (the problem then would just be there are too many entries).  That's why I used 3 pairs of submodules, not 2, to recreate more easily.
