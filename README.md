# Practical Session: Software Reproducibility - Questions

## 1.1 Comparing things

### 1.1.1 Questions

**Q1:** How likely is it that two different strings produce an identical cryptographical hash with SHA-256?  
**Answer:**


**Q2:** Think about how this algorithm works and try it with different inputs. Based on your understanding, try to come up with two different strings that produce the same checksum.  
**Answer:** 

**Q3:** Is it advised to use a tool such as TAR or Gzip to consolidate a filesystem object (e.g., symlink, file, directory) into a single file, then compute the digest of that resulting file?  
**Answer:** 

---

## 1.2 Using your local machine

### 1.2.1 Building and running a trivial C program

**Q1:** Identify the metadata of the compiled binary (e.g., size, permissions, architecture). Does the build process generate any intermediate files?  
**Answer:** *The compiled binary has a size of 16 KB, permissions set to -rwxr-xr-x, and is built for the x86_64 architecture. The build process do not generate any intermediate files, only the final binary.*

**Q2:** How does your output compare with that of other students?  
**Answer:** *The outputs are not the same because the timestamp embedded in the binary changes with each compilation.*

**Q3:** If you compile the program multiple times, do you always get the same output? Explain why (not).  
**Answer:** *No, we do not always get the same output because the timestamp embedded in the binary changes with each compilation, leading to different checksums.*

**Q4:** If you run the program multiple times (without recompiling), do you always get the same output?  
**Answer:** *Yes, we always get the same output when running the program multiple times without recompiling, as the binary contains the same embedded timestamp.*

**Q5:** If you share the output file with another student, does it work on their machine? What happens if their machine has a different architecture (e.g., Linux (x86) vs. macOS (ARM64))?  
**Answer:** *If the output file is shared with another student, it may not work on their machine if they have a different architecture. For example, a binary compiled for x86 architecture may not run on an ARM64 machine, and vice versa, due to differences in instruction sets and system calls.*

**Q6:** Is it a good practice to share the binary itself versus sharing the source code? Explain your reasoning.  
**Answer:** *It is generally better to share the source code rather than the binary. Sharing the source code allows others to compile it on their own machines, ensuring compatibility with their specific architecture and environment. Additionally, sharing the source code promotes transparency and allows others to understand how the program works, make modifications, or fix bugs if necessary. Sharing binaries can lead to issues with compatibility and may not be as secure, as it can be difficult to verify the integrity of a binary file.*

**Q7 (Challenge):** Research the SOURCE_DATE_EPOCH environment variable. Can you find a way to compile hello-world.c such that the timestamp embedded in the binary is always the same, regardless of when you run the compiler?  
**Answer:** 

---

### 1.2.2 Building a Pseudo-Random Number Generator

**Q1:** Compare your output values with those of other students. Are the sequences identical even if compiled on different distributions? Why is this behavior observed with `rand()`?  
**Answer:** 

**Q2:** If you run the program multiple times (without recompiling), do you always get the same output? Is this “randomness” reproducible?  
**Answer:** 

---

### 1.2.3 Pseudo-Random Number Generator, With a Seed

**Q1:** If you compile the program multiple times, do you always get the same output? Explain why (not).  
**Answer:** *Yes, because the seed for the random number generator is set to a fixed value (TIME(NULL)), the output will be the same every time the program is compiled, regardless of when it is compiled.*

**Q2:** If you run the program multiple times (without recompiling), do you always get the same output? Explain why (not).  
**Answer:** *No, because the seed is set to the current time (TIME(NULL)), the output will differ each time the program is run, as the seed changes with each execution.*

**Q3:** Does this version of the application behave differently (at runtime)? Explain why (not).  
**Answer:** *Yes, this version of the application behaves differently at runtime because the seed for the random number generator is based on the current time. As a result, each execution will produce a different sequence of random numbers, making the output non-reproducible across different runs.*

---

### 1.2.4 Monte Carlo pi Approximation

**Q1:** Implement the π approximation algorithm in `montecarlo-pi.c`, using the current time as the seed for the `rand()` function. Use the `srand()` function for setting this seed. Compile and execute the program to confirm that its intended behaviour is correct. If coding this program is too complex for you, use the provided template.  
**Answer:** 

**Q2:** What can you notice when you increase the number n of iterations? Does the estimate of π get closer to the actual value? How does the execution time change? Is this behaviour consistent with your expectations?  
**Answer:** 

**Q3:** Verify if the implementation is reproducible at build time (different compilations produce the same executable). Use SHA-512 checksum. Is it reproducible? Explain why (not).  
**Answer:** 

**Q4:** Verify if the implementation is reproducible at run time (different executions produce the same output). If not, explain why and provide a reproducible version.  
**Answer:** 

**Q5:** Provide a version of the program that is both build-time and run-time reproducible. Explain how you achieved this.  
**Answer:** 

---

### 1.2.5 Using Containers

**Q1:** How many parameters should your Monte Carlo application accept to be reproducible at build-time and runtime? Motivate your answer.  
**Answer:** 

**Q2:** Build the image using single-stage and multi-stage Containerfile examples, compare and report sizes. Motivate your answer.  
**Answer:** 

**Q3:** Build the image using multi-stage Containerfile and compare its size with the binary. Why such a difference? Motivate your answer.  
**Answer:** 

**Q4:** In the multi-stage Containerfile, what is the purpose of `COPY --from=build-env`? Is it good or bad? Motivate your answer.  
**Answer:** 

**Q5 (Challenge):** Work with a partner to build a container image that results in the EXACT same image ID on both machines. What obstacles did you encounter?  
**Answer:** 

**Q6:** When comparing with other students, do you all obtain the same output when running it with the same input parameters?  
**Answer:** 

**Q7:** If you share the image using `podman save` and `podman load`, what is actually being transferred? Does this ensure reproducibility?  
**Answer:** 

---

### 1.2.6 Using Nix

**Q1:** Compare the resulting binary with other students. Is it the same?  
**Answer:** 

**Q2:** Compare the installation path of the binary with other students. Is it the same?  
**Answer:** 

**Q3:** If you build it multiple times, do you get the same resulting output?  
**Answer:** 

**Q4:** What happens when you run `nix shell nixpkgs#hello`? How does it differ from `nix profile add nixpkgs#hello`?  
**Answer:** 

**Q5:** Explain the role of the Nix store (`/nix/store`) and why it is immutable.  
**Answer:** 

**Q6:** What does `nix flake lock` do, and why is it critical for reproducibility?  
**Answer:** 

**Q7:** Purity & Sandboxing: What would happen if your program tried to download a file or read `/etc/passwd` during the build? Why is this restriction important?  
**Answer:** 

**Q8:** Suppose an upstream dependency updates unexpectedly. How does Nix ensure reproducibility?  
**Answer:** 

**Q9:** You need to share a reproducible environment with Java and GCC. What would a minimal `flake.nix` look like? Should you share `flake.lock` too?  
**Answer:** 

---

## 1.3 General questions

**Q1:** What are the benefits of using Nix compared to containerisation tools like Docker or Podman?  
**Answer:** 

**Q2:** Is it safe to run a file or container image from an unknown source? What isolation mechanisms exist, and what are their limits?  
**Answer:** 

**Q3:** AI & Reproducibility: Given the same prompt and model (e.g., Llama-3), is the output always reproducible? Why or why not?  
**Answer:** 

**Q4:** If you wanted to share your Monte Carlo π estimator with a student who does not have Nix installed, how could you distribute it?  
**Answer:** 

**Q5:** Would you be interested to learn how to create PDF documents with UMons style using LaTeX or Typst?  
**Answer:** 

**Q6:** This practical session was delivered for the second time this year. What did you like or dislike about it? How would you suggest improving it?  
**Answer:** 