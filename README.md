# Practical Session: Software Reproducibility - Questions

**Note:** *I use A.I. to help me to improve my answers to the questions because the time I have to spend on this practical session is very limited. I also verify the answers I get from A.I. and I make sure they are correct and relevant to the questions. I also try to provide detailed explanations and examples to support my answers.*

## 1.1 Comparing things

### 1.1.1 Questions

**Q1:** How likely is it that two different strings produce an identical cryptographical hash with SHA-256?  
**Answer:** *No, the two different strings are not likely to produce an identical cryptographical hash with SHA-256. The probability of a collision (two different inputs producing the same hash) is extremely low due to the large output space of SHA-256 (2^256 possible hashes). However, it is not impossible, but it would require an astronomical amount of computational power to find such a collision.*


**Q2:** Think about how this algorithm works and try it with different inputs. Based on your understanding, try to come up with two different strings that produce the same checksum.  
**Answer:** *It is not feasible to come up with two different strings that produce the same checksum with SHA-256 due to the algorithm's design and the vast number of possible outputs. The best approach to find a collision would be to use a brute-force method, which is computationally infeasible given the current technology.*

**Q3:** Is it advised to use a tool such as TAR or Gzip to consolidate a filesystem object (e.g., symlink, file, directory) into a single file, then compute the digest of that resulting file?  
**Answer:** *Yes, it is generally advised to use tools like TAR or Gzip to consolidate filesystem objects into a single file before computing the digest. This ensures that the digest is computed on a consistent representation of the data, making it more reliable for verification purposes.*

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
**Answer:** *The output values are not identical across different students, even if compiled on different distributions. This behavior is observed with `rand()` because the seed for the random number generator is typically based on the current time (e.g., using `time(NULL)`), which changes with each execution. As a result, the sequence of random numbers generated will differ each time the program is run, leading to non-reproducible outputs across different runs and different machines.*

**Q2:** If you run the program multiple times (without recompiling), do you always get the same output? Is this “randomness” reproducible?  
**Answer:** *No, if you run the program multiple times without recompiling, you do not get the same output. This is because the seed for the random number generator is based on the current time, which changes with each execution. Therefore, the "randomness" is not reproducible across different runs of the program.*

---

### 1.2.3 Pseudo-Random Number Generator, With a Seed

**Q1:** If you compile the program multiple times, do you always get the same output? Explain why (not).  
**Answer:** *Yes, because the seed for the random number generator is set to a fixed value (argued as a parameter), the output will be the same each time the program is compiled and run, regardless of when it is executed. This is because the sequence of random numbers generated will be the same for the same seed value.*

**Q2:** If you run the program multiple times (without recompiling), do you always get the same output? Explain why (not).  
**Answer:** *Yes, if you run the program multiple times without recompiling, you will always get the same output because the seed for the random number generator is fixed. As long as the seed value remains unchanged, the sequence of random numbers generated will be identical across different runs of the program.*

**Q3:** Does this version of the application behave differently (at runtime)? Explain why (not).  
**Answer:** *No, this version of the application behaves the same at runtime because the seed for the random number generator is fixed. As a result, each execution will produce the same sequence of random numbers, making the output reproducible across different runs.*

---

### 1.2.4 Monte Carlo pi Approximation

**Q2:** What can you notice when you increase the number n of iterations? Does the estimate of π get closer to the actual value? How does the execution time change? Is this behaviour consistent with your expectations?  
**Answer:** *If I increase the number n of iterations, I notice that the estimate of π gets closer to the actual value. The execution time also increases as n increases, which is consistent with my expectations because more iterations require more computations to be performed.*

**Q3:** Verify if the implementation is reproducible at build time (different compilations produce the same executable). Use SHA-512 checksum. Is it reproducible? Explain why (not).  
**Answer:** *No, the sha512 checksum of the executable changes with each compilation, indicating that the implementation is not reproducible at build time. This is likely because the timestamp embedded in the binary changes with each compilation, leading to different checksums.*

**Q4:** Verify if the implementation is reproducible at run time (different executions produce the same output). If not, explain why and provide a reproducible version.  
**Answer:** *No, if I run the program multiple times, I get different outputs each time. This is because the random number generator is seeded with the current time (TIME(NULL)), which changes with each execution. To make it reproducible at runtime, I can set a fixed seed value (e.g., srand(42)) instead of using the current time. This way, the sequence of random numbers generated will be the same for every execution, resulting in the same output.*

**Q5:** Provide a version of the program that is both build-time and run-time reproducible. Explain how you achieved this.  
**Answer:** *To make the program both build-time and run-time reproducible, I can set a fixed seed for the random number generator and ensure that the compilation process does not include any variable metadata (like timestamps). For example, I can use `srand(42)` to set a fixed seed and remove the __DATE__ and __TIME__ macros from the code. This way, both the build process and the runtime behavior will be consistent and reproducible.*

---

### 1.2.5 Using Containers

**Q1:** How many parameters should your Monte Carlo application accept to be reproducible at build-time and runtime? Motivate your answer.  
**Answer:** *Only two parameters should be accepted: the seed for the random number generator and the number of iterations. This is because the seed ensures that the random number generation is reproducible at runtime, while the number of iterations allows for consistent behavior across different runs. By controlling these two parameters, we can ensure that the output is reproducible regardless of when or where the program is executed.*

**Q2:** Build the image using single-stage and multi-stage Containerfile examples, compare and report sizes. Motivate your answer.  
**Answer:** *When using a single-stage Containerfile, the resulting image is larger (278mo) while using a multi-stage approach results in a smaller image (8mo). This is because the multi-stage approach separates the build environment from the runtime environment, allowing for a smaller final image that only contains the necessary runtime dependencies.*

**Q3:** Build the image using multi-stage Containerfile and compare its size with the binary. Why such a difference? Motivate your answer.  
**Answer:** *The size of the multi-stage image (8mo) is significantly larger than the binary (16ko) because the image includes not only the compiled binary but also the necessary runtime environment and dependencies required to execute the application. The binary itself is just a small executable file, while the container image contains additional layers and files needed for the application to run properly in a containerized environment.*

**Q4:** In the multi-stage Containerfile, what is the purpose of `COPY --from=build-env`? Is it good or bad? Motivate your answer.  
**Answer:** *The `COPY --from=build-env` command is used to copy the compiled binary from the build stage (named `build-env`) to the final image. This is a good practice because it allows us to separate the build environment from the runtime environment, resulting in a smaller and more efficient final image. By only copying the necessary files, we can optimize the image size of the final image.*

**Q5 (Challenge):** Work with a partner to build a container image that results in the EXACT same image ID on both machines. What obstacles did you encounter?  
**Answer:** *It's very difficult because my partner use ARM64 architecture while I use x86_64 architecture, so the resulting image ID will be different due to the differences in the underlying architecture. Additionally, even if we use the same Containerfile and build context, the image ID can still differ due to factors such as timestamps, build environment, and other metadata that may be included in the image.*

**Q6:** When comparing with other students, do you all obtain the same output when running it with the same input parameters?  
**Answer:** *Yes, when running the container with the same input parameters, we all obtain the same output. This is because the containerization process ensures that the application runs in a consistent environment, regardless of the underlying host system. By using the same image and input parameters, we can achieve reproducibility across different machines.*

**Q7:** If you share the image using `podman save` and `podman load`, what is actually being transferred? Does this ensure reproducibility?  
**Answer:** *When sharing a container image using `podman save` and `podman load`, the actual files being transferred are the layers of the image, which include the filesystem changes and metadata. This ensures reproducibility as long as the image is not modified after being saved.*

---

### 1.2.6 Using Nix

**Q1:** Compare the resulting binary with other students. Is it the same?  
**Answer:** *Yes, the resulting binary is the same across different students because Nix ensures that the build process is deterministic and reproducible. By using a fixed set of dependencies and a consistent build environment, Nix guarantees that the same source code will produce the same binary output regardless of when or where it is built.*

**Q2:** Compare the installation path of the binary with other students. Is it the same?  
**Answer:** *Yes, the installation path is the same across different students because Nix ensures a consistent and predictable installation process. The binary is installed in a fixed location within the Nix store, which is consistent across different machines.*

**Q3:** If you build it multiple times, do you get the same resulting output?  
**Answer:** *Yes, if you build the same Nix expression multiple times, you will get the same resulting output. This is because Nix ensures that the build process is deterministic and reproducible.*

**Q4:** What happens when you run `nix shell nixpkgs#hello`? How does it differ from `nix profile add nixpkgs#hello`?  
**Answer:** *When you run `nix shell nixpkgs#hello`, it creates a temporary environment with the specified package available. In contrast, `nix profile add nixpkgs#hello` adds the package to your user profile, making it available for future sessions.*

**Q5:** Explain the role of the Nix store (`/nix/store`) and why it is immutable.  
**Answer:** *The Nix store is a directory where all files managed by Nix are stored. It is immutable because once a file is added to the store, its content cannot be changed. This ensures that builds are reproducible and that different versions of the same package can coexist without conflicts.*

**Q6:** What does `nix flake lock` do, and why is it critical for reproducibility?  
**Answer:** *`nix flake lock` generates a `flake.lock` file that captures the exact versions of all dependencies used in a Nix flake. This is critical for reproducibility because it ensures that the same versions of dependencies are used every time the flake is built, regardless of changes in the upstream repositories.*

**Q7:** Purity & Sandboxing: What would happen if your program tried to download a file or read `/etc/passwd` during the build? Why is this restriction important?  
**Answer:** *If your program tried to download a file or read `/etc/passwd` during the build, it would fail because Nix builds are sandboxed and do not have access to the network or the host filesystem. This restriction is important because it ensures that builds are reproducible and isolated from external factors, preventing unintended side effects and ensuring that the build process is deterministic.*

**Q8:** Suppose an upstream dependency updates unexpectedly. How does Nix ensure reproducibility?  
**Answer:** *Nix ensures reproducibility by using the `flake.lock` file, which captures the exact versions of all dependencies. Even if an upstream dependency updates unexpectedly, as long as the `flake.lock` file is not updated, Nix will continue to use the same versions of dependencies, ensuring that builds remain reproducible.*

**Q9:** You need to share a reproducible environment with Java and GCC. What would a minimal `flake.nix` look like? Should you share `flake.lock` too?  
**Answer:** *A minimal `flake.nix` for sharing a reproducible environment with Java and GCC could look like this:*

```nix
{
  description = "A reproducible environment with Java and GCC";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs";
  };

  outputs = { self, nixpkgs }: {
    devShell.x86_64-linux = nixpkgs.legacyPackages.x86_64-linux.mkShell {
      buildInputs = [
        nixpkgs.legacyPackages.x86_64-linux.openjdk
        nixpkgs.legacyPackages.x86_64-linux.gcc
      ];
    };
  };
}
```

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