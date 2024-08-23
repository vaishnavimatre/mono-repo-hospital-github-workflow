SERVICES  = directory-service case-service patient-service appointment-service
LIB_PROJECT = javafx-library
DOCKER_REPO = vaish864

all: clone git_status git_pull git_stash build_lib build_services build_images push_images

clone:
	@echo "Cloning all services and JavaFX project..."
	@for service in $(SERVICES); do \
		if test ! -d $$service; then \
			git clone https://github.com/vaishnavimatre/mono-repo-hospital-management.git hospital-management/$$service; \
		else \
			echo "$$service already exists."; \
		fi \
	done
	@if test ! -d $(LIB_PROJECT); then \
		git clone https://github.com/vaishnavimatre/mono-repo-hospital-management.git hospital-management/$(LIB_PROJECT); \
	else \
		echo "$(LIB_PROJECT) already exists."; \
	fi

git_status:
	@echo "Getting git status for all projects..."
	@for project in $(SERVICES) $(LIB_PROJECT); do \
		(cd $$project && git status); \
	done

git_pull:
	@echo "Pulling latest changes for all projects..."
	@for project in $(SERVICES) $(LIB_PROJECT); do \
		(cd $$project && git pull); \
	done

git_stash:
	@echo "Stashing changes in all projects..."
	@for project in $(SERVICES) $(LIB_PROJECT); do \
		(cd $$project && git stash); \
	done

build_lib: 
	@echo "Building JavaFX library..."
	@(cd $(LIB_PROJECT) && mvn clean install)

build_services:
	@echo "Building all services..."
	@for service in $(SERVICES); do \
		(cd $$service && mvn clean package); \
	done

build_images:
	@echo "Building Docker images for all services..."
	@for service in $(SERVICES); do \
		docker build -t $(DOCKER_REPO)/$$service:latest $$service; \
	done

push_images: 
	@echo "Pushing all Docker images..."
	@for service in $(SERVICES); do \
		docker push $(DOCKER_REPO)/$$service:latest; \
	done
