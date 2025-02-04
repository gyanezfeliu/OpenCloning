# Readme

## Intro

OpenCloning is a web application to generate molecular cloning strategies in json format, and share them with others. It is the first brick in the construction of [Genestorian](https://www.genestorian.org/), a web application to manage collections of plasmids, strains and cell lines.

You can find a demo video of the application at https://www.youtube.com/watch?v=n0hedzvpW88

## :timer_clock: Getting started in 5 minutes

If you just want to try the application, the easiest way is to go to the hosted version at: [https://opencloning.org/](https://opencloning.org/).

If you want to quickly set up a local instance of the application using docker, you can clone this repository, and from the root directory call:

```bash
# see docker-compose.yml
docker-compose up
```

The application should be running at [http://localhost:8000](http://localhost:8000).

This uses the provided `docker-compose.yaml` and `config.json` files in this repository.

## Running it yourself using docker in a single container

You can use the image [manulera/opencloning](https://hub.docker.com/r/manulera/opencloning), and use `docker-compose.yaml` as a starting point. The important information to know is that:

* The image exposes port 8000.
* You can allow requests to the API from origins other than the frontend via CORS using ENV variables, see the [backend configuration](https://github.com/manulera/OpenCloning_backend#connecting-to-the-frontend). See the comments in the `docker-compose.yaml` file.
* This container serves both the frontend and the backend, but you can run them as separate containers (will need CORS configuration).

```bash
docker pull manulera/opencloning
docker run -p 8000:8000 manulera/opencloning
```

### Running an instance in GitHub Codespaces

* Click the menu at the top left of the github website, then click codespaces
* On the top-right click on `New codespace`
* On `Repository` select `manulera/OpenCloning`
* Click on `Create codespace`
* In the terminal enter `docker-compose up`
* Once complete a popup will appear saying port 8000 is now available, click `Open in Browser` to navigate to your own version of the OpenCloning app
* Optional: To share with other users, navigate to the ports tab of the github codespace and right click and change port visibility to public


## Running it yourself and configuration

If you want to run a dev server locally, or run the site without using docker, you can see how to set up the backend and frontend in their respective repositories:

* [Frontend](https://github.com/manulera/OpenCloning_frontend)
* [Backend](https://github.com/manulera/OpenCloning_backend)

### Backend

The code of the backend is here: [https://github.com/manulera/OpenCloning_backend](https://github.com/manulera/OpenCloning_backend)

The backend is a web API built with FastAPI. For information on what it does, and how to install it and what it does see [this](https://github.com/manulera/OpenCloning_backend).

### Frontend

The code of the frontend is here: [https://github.com/manulera/OpenCloning_frontend](https://github.com/manulera/OpenCloning_frontend)

The frontend application is built with react, and it is the "family tree builder" that you will see in your browser. For more info on what it does and how to install it see [this](https://github.com/manulera/OpenCloning_frontend)

## About

### :dna: Biological background

Recombinant DNA technology is used in a variety of research and industry fields to generate new DNA molecules by combining fragments of existing ones. This means that every molecule in a laboratory collection was created by "cutting and pasting" the sequences of existing molecules.

The aim of this application is to provide a web interface to document the generation of new DNA molecules from existing ones, and to export this information to share it with others. You can imagine it as a family tree builder, where there are two kinds of entities:

1. The DNA molecules, or `entities`.
2. The `sources`, which are experimental steps that take 0 or more `entities` as an input, and generate a single output. There can be two kinds of `sources`:
	1.	**`Sources` without a parent `entity`:** They represent the source of a DNA molecule received externally (e.g., a plasmid received from a collaborator or from Addgene) or a naturally occurring sequence (e.g., given by an assembly identifier and genome coordinates).
	2. **`Sources` representing cloning steps combining existing `entities` to generate new `entities`:** They contain references to the input and output DNA sequences, the method name (digestion, ligation, etc.) and the minimal information to do the cloning step in silico.

See the figure below for an example of PCR-based gene targeting, in which a fragment of a plasmid is amplified by PCR with primers that contain 5' extensions homologous to target sequences in the genome. Cells are then transformed with the PCR fragment, which integrates into the genome through homologous recombination.

![](cloning.drawio.svg)

You can see how the workflow of cloning happens in the app in [this video](https://www.youtube.com/watch?v=n0hedzvpW88&ab_channel=Genestorian)

### Encoding this information

The data model is built using the [LinkML](https://linkml.io/) framework, and can be accessed in [this repository](https://github.com/genestorian/OpenCloning_LinkML).

For an example of the data model to represent an homologous recombination, you can see [this json file](https://github.com/manulera/OpenCloning_frontend/blob/master/public/examples/homologous_recombination.json).

From the json, you can see how wvery `entity` (a sequence) comes from a `source`, and every `entity` can be the input of another `source`. The application frontend provides an interface where the user can specify a `source` (with or without inputs). This `source` is sent to the backend in a `POST` request, where the step encoded in the `source` is executed, and the output `entity` is returned and displayed in the frontend. When multiple outputs could come out of a `source` (for example, a restriction enzyme digestion), the user can select which one of them is the desired output. Then the user can use the output `entity` as an input for a new `source`, and so on.

## Contributing

If you are interested in contributing, check the [contribution guidelines](CONTRIBUTING.md).

## Acknowledgements :pray:

Thanks to [@ikayz](https://github.com/ikayz) for initial improvements to the frontend.

Thanks to [@joyceykao](https://github.com/joyceykao) for discussions on how to approach UI/UX and users interview.

Thanks to [@maratumba](https://github.com/maratumba) for recommending the usage of FastAPI and for giving some general guidelines for the development of the backend and hosting.

Thanks to the whole [Open Life Science](https://openlifesci.org/) organising team and community, which were incredibly helpful in setting the foundations of this project. Special thanks to [@DimmestP](https://github.com/DimmestP), who mentored [@manulera](https://github.com/DimmestP) during the program [OLS-4](https://openlifesci.org/ols-4).
