# ProgrammingAssignment2

R version 4.4.1 (2024-06-14 ucrt) -- "Race for Your Life"
Copyright (C) 2024 The R Foundation for Statistical Computing
Platform: x86_64-w64-mingw32/x64

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

[Workspace loaded from ~/.RData]

> makeVector <- function(x = numeric()) {
+     m <- NULL  # This will store the cached mean
+     
+     set <- function(y) {
+         x <<- y  # Assigns y to x in the parent environment
+         m <<- NULL  # Resets the cached mean
+     }
+     
+     get <- function() x  # Returns the stored vector
+     
+     setmean <- function(mean) m <<- mean  # Stores the mean in cache
+     
+     getmean <- function() m  # Retrieves the cached mean
+     
+     # Return a list of functions to interact with this special "vector"
+     list(set = set, get = get, setmean = setmean, getmean = getmean)
+ }
> 
> vec <- makeVector(c(1, 2, 3, 4, 5))
> 
> vec$get()  # Returns: c(1, 2, 3, 4, 5)
[1] 1 2 3 4 5
> vec$setmean(mean(vec$get()))  # Stores the mean
> vec$getmean()  # Retrieves cached mean: 3
[1] 3
> 
> makeCacheMatrix <- function(x = matrix()) {
+     inv <- NULL  # Store the cached inverse
+     
+     # Set the matrix and reset cache
+     set <- function(y) {
+         x <<- y
+         inv <<- NULL  # Reset cached inverse
+     }
+     
+     # Get the matrix
+     get <- function() x
+     
+     # Set the inverse (cache it)
+     setInverse <- function(inverse) inv <<- inverse
+     
+     # Get the cached inverse
+     getInverse <- function() inv
+     
+     # Return a list of functions
+     list(set = set, get = get, setInverse = setInverse, getInverse = getInverse)
+ }
> 
> cacheSolve <- function(x, ...) {
+     inv <- x$getInverse()  # Check if inverse is already cached
+     
+     if (!is.null(inv)) {  # If cached, return it
+         message("Getting cached inverse")
+         return(inv)
+     }
+     
+     # Compute inverse if not cached
+     mat <- x$get()
+     inv <- solve(mat, ...)  # Compute inverse using solve()
+     x$setInverse(inv)  # Cache the inverse
+     
+     inv  # Return the inverse
+ }
> 
> # Create a matrix
> my_matrix <- matrix(c(4, 7, 2, 6), nrow = 2, ncol = 2)
> 
> # Create a special matrix object
> cachedMatrix <- makeCacheMatrix(my_matrix)
> 
> # Compute and cache the inverse
> inverse1 <- cacheSolve(cachedMatrix)  # First time: computes inverse
> 
> # Retrieve the cached inverse
> inverse2 <- cacheSolve(cachedMatrix)  # Second time: gets cached inverse
Getting cached inverse
> 
> # Display results
> print(inverse1)
     [,1] [,2]
[1,]  0.6 -0.2
[2,] -0.7  0.4
> print(inverse2)
     [,1] [,2]
[1,]  0.6 -0.2
[2,] -0.7  0.4
> 
