---
notion_id: 3bf4fa76-9938-80f7-98d8-ffe2579d1bc7
notion_url: https://app.notion.com/p/Docker-for-ML-Models-3bf4fa76993880f798d8ffe2579d1bc7
title: Docker for ML Models
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-17T15:03:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-19T00:37:55.771Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## What is docker?


Docker is a platform designed to help developers build, run and share container applications


## Why do we need it ?

- Consistency across env:-
    - eg:- maggi noodles →noodles (first part), masala(second part)

        what if a recipe book came instead of masala and we had to create that at home by ourselves then the problem is the taste of maggi would have been different everywhere 


        in software also it can happen where the os and the library can be different at different stages of the development like testing , developing and deploying so people thought to store the code at developing time in a container and send that container to various stages instead of software

- Isolation
    - running multiple applications on the same host can lead to conflicts, such as dependency clashes or resource contention
    - solution: docker provides isolated env for each application, preventing interference and ensuring stable performances
- Scalability
    - Problem:- Scaling applications to handle increased load can be challenging, requiring manual intervention and configuration
    - Solution: Docker makes it easy to scale them horizontally by running multiple container instances, allowing for quick and efficient scaling.

## How are they used?


[link_preview](https://drive.google.com/file/d/1AGcGb49pU55Dm2B7aBRBgaTKMNrBIXm6/view?usp=sharing)


Docker file is a text file that is there to build the image and the image when executed on a host creates an instance which is known as docker container


there is a box which is the image and when we open the box and use it that is container
