- ```javascript
  function workLoop(isYieldy) {
    if (!isYieldy) {
      while (nextUnitOfWork !== null) {
        nextUnitOfWork = performUnitOfWork(nextUnitOfWork);
      }
    } else {...}
  }
  function performUnitOfWork(workInProgress) {
      let next = beginWork(workInProgress);
      if (next === null) {
          next = completeUnitOfWork(workInProgress);
      }
      return next;
  }
  
  function beginWork(current$$1, workInProgress) {
    switch (workInProgress.tag) {
      ...
  	case FunctionalComponent: {...}
  	case ClassComponent: {
  		...
  		return updateClassComponent(current$$1, workInProgress, ...);
  	}
  	case HostComponent: {...}
      return workInProgress.child;
  }
                                 
  function updateClassComponent(current, workInProgress, Component, ...) {
      ...
      const instance = workInProgress.stateNode;
      let shouldUpdate;
      if (instance === null) {
          ...
          // In the initial pass we might need to construct the instance.
          constructClassInstance(workInProgress, Component, ...);
          mountClassInstance(workInProgress, Component, ...);
          shouldUpdate = true;
      } else if (current === null) {
          // In a resume, we'll already have an instance we can reuse.
          shouldUpdate = resumeMountClassInstance(workInProgress, Component, ...);
      } else {
          shouldUpdate = updateClassInstance(current, workInProgress, ...);
      }
      return finishClassComponent(current, workInProgress, Component, shouldUpdate, ...);
  }
  
  function updateClassInstance(current, workInProgress, ctor, newProps, ...) {
      const instance = workInProgress.stateNode;
  
      const oldProps = workInProgress.memoizedProps;
      instance.props = oldProps;
      if (oldProps !== newProps) {
          callComponentWillReceiveProps(workInProgress, instance, newProps, ...);
      }
  
      let updateQueue = workInProgress.updateQueue;
      if (updateQueue !== null) {
          processUpdateQueue(workInProgress, updateQueue, ...);
          newState = workInProgress.memoizedState;
      }
  
      applyDerivedStateFromProps(workInProgress, ...);
      newState = workInProgress.memoizedState;
  
      const shouldUpdate = checkShouldComponentUpdate(workInProgress, ctor, ...);
      if (shouldUpdate) {
          instance.componentWillUpdate(newProps, newState, nextContext);
          workInProgress.effectTag |= Update;
          workInProgress.effectTag |= Snapshot;
      }
  
      instance.props = newProps;
      instance.state = newState;
  
      return shouldUpdate;
  }
                                 
  function completeUnitOfWork(workInProgress) {
      while (true) {
          let returnFiber = workInProgress.return;
          let siblingFiber = workInProgress.sibling;
  
          nextUnitOfWork = completeWork(workInProgress);
  
          if (siblingFiber !== null) {
              // If there is a sibling, return it
              // to perform work for this sibling
              return siblingFiber;
          } else if (returnFiber !== null) {
              // If there's no more work in this returnFiber,
              // continue the loop to complete the parent.
              workInProgress = returnFiber;
              continue;
          } else {
              // We've reached the root.
              return null;
          }
      }
  }
  
  function completeWork(workInProgress) {
      console.log('work completed for ' + workInProgress.name);
      return null;
  }
  ```